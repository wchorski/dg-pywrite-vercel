---
{"dg-publish":true,"tags":["javascript"],"permalink":"/developer/NextJS/Google API Calendar With NextJS App/","dgPassFrontmatter":true}
---

Most tutorials dealing with the [Google Calendar API](https://developers.google.com/calendar/api/quickstart/nodejs) deal with your users loggin in to add the created event to their personal calendars. In my case I have employees that I want to create/ update calendars events for a single *main* calendar, without having any login. Authorization is already set up and calendar is pointed during development
## Lightbulb Moment

I was still struggling on how the flow of information happens since most of the data is moved in a Headless environment. [link](https://zappysys.com/api/integration-hub/google-calendar-connector/help/authentication/ServiceAccount#:~:text=Copy the email address of your service account,On the left%2C find the "My calendars" section.) helped me understand how to set it up. Here is the bullet points.

(this assumes you've already set up the project & Calendar APIs via the google console)

- Log into your Google Account you conduct regular buisness `MYBUISNESS@gmail.com`
- Create (or view an existing email)
- Go to share settings of selected calendar 
- Add the generated **Service Account** email (will look something like `MYBUISNESS-project.iam.gserviceaccount.com`)
- Make sure permissions are set correctly (in my case I set it to "Make changes and manage sharing")
- On the same page, copy the calendar  (example: `bda948b786f16be2d763e0b2d3c699bc8230643f96bfbfb2at64b66e7fc92b11@group.calendar.google.com`)

## NextJS API Route

I'm wrapping the google api in a [[developer/NextJS/NextJS\|NextJS]] API route so that everything happens on the server

`/pages/api/google/create.ts`
```ts
import type { NextApiRequest, NextApiResponse } from 'next'

import { google } from 'googleapis';
import { JWT } from 'google-auth-library';
import creds from "../../../private/cutefruit-project.json";

const scopes = ['https://www.googleapis.com/auth/calendar'];
const GOOGLE_PRIVATE_KEY = creds.private_key|| 'NO_KEY_SET'
const GOOGLE_CLIENT_EMAIL = creds.client_email || 'NO_EMAIL_SET'
const GOOGLE_CAL_ID = process.env.GOOGLE_CAL_ID || 'primary'


let jwtClient = new JWT({
  email: GOOGLE_CLIENT_EMAIL,
  key: GOOGLE_PRIVATE_KEY,
  scopes,
})

jwtClient.authorize(function (err, tokens) {
  if (err) {
    console.log(err);
    return
  } else {
    console.log("Successfully connected to googleapi via JWT!")
  }
})

type Data = {
  id: string|undefined|null,
  htmlLink: string|undefined|null,
  kind: string|undefined|null,
  status: string|undefined|null,
  message: any,
}


export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse<Data>
) {
  // console.log(req.method)
  if(req.method === 'POST'){
    const event = req.body
    // console.log({event})
    
    let calendar = google.calendar('v3')
    
    try {
      const response = await calendar.events.insert({
        auth: jwtClient,
        calendarId: GOOGLE_CAL_ID,
        requestBody: event,
      })
      res.status(response.status).json({ 
        id: response.data.id,
        htmlLink: response.data.htmlLink, 
        kind: response.data.kind,
        status: response.data.status,
        message: response.statusText, 
      })
      
    } catch (err:any) {
      console.log('Google Cal API Error: ' + err)

      res.status(201).json({ 
        id: undefined,
        htmlLink: undefined, 
        kind: undefined,
        status: undefined,
        message: err.errors.map((err:any) =>  err.message).join(', ') 
      })
    }
    

    // res.status(200).json({ message: 'default response' })
  }
}

```

## Usage

with this snippet you can retrieve anything created in this calendar

```ts
calendar.events.list({
		auth: jwtClient,
		calendarId: GOOGLE_CAL_ID
	}, function (err, response) {

		if(response) {
			console.log('----- here is a list of events in this calendar ---- ');
			
			return console.log(response.data.items)
		};
		if(err) return console.log(err);
		
	})
```