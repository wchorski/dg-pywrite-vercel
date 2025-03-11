---
{"dg-publish":true,"tags":["docker","docker-compose","linux","automation","backup"],"permalink":"/developer/Docker🐳/Update Database Containers and Others Reliably/","dgPassFrontmatter":true}
---

## Containers that don't have a built in script
1. Stop the application (`docker-compose down`)
2. Start just the database (`docker-compose up -d db`)
3. Export all databases on the server into plaintext (`pg_dumpall -U username > data.sql`)
4. Stop the database (`docker-compose down`)
5. Remove the contents of the database directory, wherever you’ve mounted in `/var/lib/postgres`
6. Update the tag you are using, and pull the new container (`postgres:12-alpine` → `postgres:14-alpine`)
7. Start just the database (`docker-compose up -d db`). This will recreate a fresh database, using the credentials from the environment.
8. Import the data into the new database (`psql -U username < data.sql`)
9. Stop the database container (`docker-compose down`)
10. Start everything (`docker-compose up -d`)

---
## Credits
- [Upgrading Databases in Docker :: TheOrangeOne](https://theorangeone.net/posts/upgrading-docker-databases/)