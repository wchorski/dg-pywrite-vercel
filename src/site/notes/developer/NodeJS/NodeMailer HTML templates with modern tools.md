---
{"dg-publish":true,"tags":["reactjs","nodejs","email"],"permalink":"/developer/node-js/node-mailer-html-templates-with-modern-tools/","dgPassFrontmatter":true}
---

haven't tried this out myself, but looks like it makes sense, but I'd try out [Send email using Nodemailer - React Email](https://react.email/docs/integrations/nodemailer) first

```jsx
const React = require('react');
const ReactDOMServer = require('react-dom/server');
const nodemailer = require('nodemailer');

// Your React component with styles
const MyStyledComponent = () => {
  const styles = {
    container: {
      fontFamily: 'Arial, sans-serif',
      backgroundColor: '#f0f0f0',
      padding: '20px',
      textAlign: 'center',
    },
    heading: {
      color: '#333',
    },
    paragraph: {
      color: '#555',
    },
  };

  return (
    <div style={styles.container}>
      <h1 style={styles.heading}>Hello, World!</h1>
      <p style={styles.paragraph}>This is a sample email content with inline styles.</p>
    </div>
  );
};

// Render the React component to static markup
const htmlString = ReactDOMServer.renderToStaticMarkup(<MyStyledComponent />);

// Create a Nodemailer transporter
const transporter = nodemailer.createTransport({
  // Your email configuration goes here
  service: 'gmail',
  auth: {
    user: 'your-email@gmail.com',
    pass: 'your-email-password',
  },
});

// Email options
const mailOptions = {
  from: 'your-email@gmail.com',
  to: 'recipient@example.com',
  subject: 'Subject of the email',
  html: htmlString, // Set the HTML content here
};

// Send the email
transporter.sendMail(mailOptions, (error, info) => {
  if (error) {
    console.error(error);
  } else {
    console.log('Email sent: ' + info.response);
  }
});

```

## React Email Integration
This looks a bit more promising

[Send email using Nodemailer - React Email](https://react.email/docs/integrations/nodemailer)

email.jsx
```jsx
import * as React from 'react';
import { Html } from '@react-email/html';
import { Button } from '@react-email/button';

export function Email(props) {
  const { url } = props;

  return (
    <Html lang="en">
      <Button href={url}>Click me</Button>
    </Html>
  );
}

```

nodemailer.ts
```ts
import { render } from '@react-email/render';
import nodemailer from 'nodemailer';
import { Email } from './email';

const transporter = nodemailer.createTransport({
  host: 'smtp.forwardemail.net',
  port: 465,
  secure: true,
  auth: {
    user: 'my_user',
    pass: 'my_password',
  },
});

const emailHtml = render(Email({ url: "https://example.com" }));

const options = {
  from: 'you@example.com',
  to: 'user@gmail.com',
  subject: 'hello world',
  html: emailHtml,
};

await transporter.sendMail(options);

```
## Backlinks
- [[developer/NodeJS/NodeJS\|NodeJS]]