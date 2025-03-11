---
{"dg-publish":true,"permalink":"/developer/Docker🐳/Docker, Limit Storage Space/","dgPassFrontmatter":true}
---

You can use the `storage_opt` key in the Docker Compose file to set storage size limits for containers.

Here's an example `docker-compose.yml` file that sets a storage size limit of 10GB for a container:

```yml
version: '3'

services:
  myservice:
    image: myimage
    storage_opt:
      size: 10G
```

In this example, `myservice` is the name of the service you want to create, `myimage` is the name of the Docker image you want to use for the service, and `storage_opt` is the key that specifies the storage options for the container.

You can also set other storage options in the `storage_opt` key, such as the `dm.basesize` option to set the base size of the container's thin pool volume. Here's an example that sets both the size and base size options:

```yml
version: '3'

services:
  myservice:
    image: myimage
    storage_opt:
      size: 10G
      dm.basesize: 5G
```

In this example, the container will have a storage size limit of 10GB and a base size of 5GB for the thin pool volume.

---
## Credits
- [Using Docker-Compose with storage options (size) - Stack Overflow ](https://stackoverflow.com/questions/41521832/using-docker-compose-with-storage-options-size)
- Chat GPT