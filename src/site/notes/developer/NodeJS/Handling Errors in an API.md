---
{"dg-publish":true,"permalink":"/developer/node-js/handling-errors-in-an-api/","dgPassFrontmatter":true}
---

Making an API that that returns logical responses can be a bit confusing. So I'm gonna write out an example lifted from one of my projects using [[developer/NextJS/NextJS\|NextJS]] and [[developer/Home Lab/CouchDB\|CouchDB]] 

This script creates default users if none are found in the database. This helps automate the initial build.

api call `pages/api/users/default.js`
```js
/**
 * @param {import('next').NextApiRequest} req
 * @param {import('next').NextApiResponse} res
 */

import * as Nano  from 'nano'
const bcrypt = require('bcrypt');
import usersJSON from '../../../js/defaultUsers.json'
import ROLES from '../../../js/roles_list'

const protocol = process.env.DB_PROTOCOL
const dbAdminName = process.env.DB_ADMIN_NAME
const dbAdminPass = process.env.DB_ADMIN_PASSWORD
const dbUrl = process.env.DB_ENDPOINT

export default async function defaultUser(req, res) {

  const dbEndpoint = protocol + dbAdminName + ':' + dbAdminPass + '@' + dbUrl

  try {
    const nano = Nano(dbEndpoint)
    const db = nano.db.use('users')
    const info = await db.info()

    const docCount = info.doc_count

    if(docCount === 0){
      const nano = Nano(dbEndpoint)
      const db = nano.db.use('users')
      
      console.log('-- NO USERS FOUND --');
      console.log('-- CREATING DEFAULT --');

      usersJSON.map(async (usr) => {

        const { password, roles, img } = usr

        const hashedPwd = await bcrypt.hash(password, 10);

        const user = {
          ...usr,
          img: (img) ? img : '/imgs/partyhat.svg"',
          roles: (roles) ? roles : {client: ROLES.client},
          password: hashedPwd,
          dateCreated: Date.now(),
          dateMod: Date.now()
        }

        await db.insert(user, `user:${user.email}`, function(err, body) {
          if (err) {
            console.warn('couchdb error: ',err)
          } else {
            console.log('##### DEFAULT USER CREATED', body);
          }
        });
      })

	  // make sure this res is outside the `.map` (or any looping function)
      return res.status(201).json({ status: 201, message: 'Default Users Created' })
    }

    if(docCount > 0) return res.status(202).json({ status: 202, message: 'Default Users Already Exist' })

    return res.status(202).json({ status: 202, message: 'something else happened successfully with users' })

  } catch (err) {
    console.error('connect to users db: ',err)
    return res.status(400).json({ status: 'failed info', message: err.toString() })
  }
  
}
```

---
## Credits
- [Handling API errors in Next.js (giancarlobuomprisco.com)](https://giancarlobuomprisco.com/next/handling-api-errors-in-nextjs)