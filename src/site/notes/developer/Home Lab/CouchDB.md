---
{"dg-publish":true,"permalink":"/developer/home-lab/couch-db/","dgPassFrontmatter":true}
---

## [Apache CouchDB](https://couchdb.apache.org/)
**Seamless** multi-master sync, that scales from **Big Data** to **Mobile,** with an **Intuitive** HTTP/JSON API and designed for **Reliability.** 

So far I've only used this for [[../Obsidian-Livesync.md\|Obsidian-Livesync]] but knowing Apache, this DB is a solid #FOSS choice

---

## start stop service
Windows → the service is called `Apache CouchDB`


### connections
- [[../Obsidian-Livesync.md\|Obsidian-Livesync]]

### installation
1. [[developer/Home Lab/Docker\|Docker]] `compose.yml`
```yaml
services:
  couchserver:
    image: couchdb
    restart: always
    ports:
      - "5984:5984"
    environment:
      - COUCHDB_USER=admin
      - COUCHDB_PASSWORD=YOURPASSWORD
    volumes:
        - ./dbdata:/opt/couchdb/data
```
---
### Enable CORs
`http://localhost:5984/_utils/#_config/couchdb@localhost/cors`


## use 127.0.0.1 not ~~localhost~~
sometimes `localhost` doesn't work because of your `host` file, so use `127.0.0.1` anytime your using the local install of **CouchDB** 

In Node 16, `localhost` ===> `127.0.0.1`  
In Node 18, `localhost` ===> `::1`, which is an IPv6 address

Note that in a default Mac `/etc/hosts` file there are two references to `localhost`:

```
127.0.0.1          localhost
::1                localhost
```

So it appears that in Node 18+, it's preferring the IPv6 option.

i.e. if you change your code to

const nano = require("nano")(`http://${user}:${pass}@127.0.0.1:5984`);

then it'll work.

## check if database exists


## default user JSON
```json
{
 "id": "org.couchdb.user:john",
 "key": "org.couchdb.user:john",
 "value": {
  "rev": "1-46853c46b6d82c12e39876789ab314d8"
 },
 "doc": {
  "_id": "org.couchdb.user:john",
  "_rev": "1-46853c46b6d82c12e39876789ab314d8",
  "name": "john",
  "roles": [],
  "type": "user",
  "password_scheme": "pbkdf2",
  "iterations": 10,
  "derived_key": "56f076cb6e9d485691a886ce053d33b0a7008046",
  "salt": "2b6cec2e790348977d4474ae71afd60b"
 }
}
```

## Design Document
- [How to retrieve all the documents in couchdb by given key? - Stack Overflow](https://stackoverflow.com/questions/28945405/how-to-retrieve-all-the-documents-in-couchdb-by-given-key)
---
## backlinks
[[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]]

---

## citations
- [link](https://docs.couchbase.com/server/current/install/startup-shutdown.html#:~:text=Windows 1 Open the Services app. You can,as whether it starts at system boot. )
- [link](https://mircozeiss.com/couchdb-security-and-pouchdb-authentication#:~:text=Make CouchDB accessible from client Use nano to,{ %2F%2F you'll need the headers object })%3B)
- [couchdb-cookie-auth/app.js at master · zemirco/couchdb-cookie-auth (github.com)](https://github.com/zemirco/couchdb-cookie-auth/blob/master/app.js)
- [Creating regular users in CouchDB - Stack Overflow](https://stackoverflow.com/questions/3684749/creating-regular-users-in-couchdb)
- https://github.com/apache/couchdb-nano/issues/313#issuecomment-1321691678
- [link](https://github.com/apache/couchdb-nano#nanodbinfocallback)


#todo 
- [ ] compose_code