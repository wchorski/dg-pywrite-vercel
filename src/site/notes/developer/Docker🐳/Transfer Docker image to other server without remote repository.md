---
{"tags":["nodejs","reactjs","docker","docker-compose","webdev"],"dg-publish":true,"permalink":"/developer/Docker🐳/Transfer Docker image to other server without remote repository/","dgPassFrontmatter":true}
---

I build my websites with [[developer/NodeJS/NodeJS\|NodeJS]] and create production ready apps with [[developer/Home Lab/Docker\|Docker]]. These images are usually unique the client and contain sensitive information. 

My production server is an [[developer/Hardware/Intel NUC\|Intel NUC]]. Enough power to drive a few websites, but chugs along when I try to build an image. Why not build the image on my [[developer/Hardware/MacBook Pro M1 Laptop\|MacBook Pro M1 Laptop]] or [[developer/Hardware/Ryzen 5 5600x Custom PC\|Ryzen 5 5600x Custom PC]] with ease, then transfer the image over?
## tutorial

In order to transfer a Docker image from one server to another, what you need to do is first export the image to a file, then copy that file over from your current server to the new one using `scp` or `rsync` and finally load the image to your new server. Here’s how to do that:

- First SSH to your server where you’ve got your Docker image currently stored at and then run the following command to generate the tar file:

```
sudo docker save -o /home/sammy/your_image.tar your_image_name
```

Copy

- This will create a `tar` file of your image and store it at the `/home/==sammy==/==your_image==.tar` folder. You can check the size of the file with the `ls` command:

```
ls -lah /home/sammy/your_image.tar
```

Copy

- Once you have your Docker image ready, you need to copy it over using `rsync`, `scp`, or [SFTP using Filezilla](https://www.digitalocean.com/docs/droplets/how-to/transfer-files/) for example. My personal favorite is `rsync`. If you are not familiar with `rsync`, I strongly suggest going through this tutorial here on how to use `rsync`:

[https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories](https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories)

In case that you decide to use `rsync` as well, to copy the file over run the following command:

```
rsync -avz /home/sammy/your_image.tar username@remote_server_ip_address:destination_directory
```

> [!note] 
> make sure to change the `destination_directory`, the `username` and the `remote_server_ip_address` with your actual details

- Once you have your `.tar` file copied over to your new server, SSH to the new server and load the Docker image:

```
sudo docker load -i your_image.tar 
```

Copy

- Then, in order to check if this was successful, you can run `docker images` to see the list of the available images:

```
sudo docker images
```


---
## Credits
- [How to copy a Docker image from one server to another without pushing it to a repository first? (youtube.com) ](https://www.youtube.com/watch?v=6jVGWHYs0B0)
- [How to copy a Docker image from one server to another without pushing it to a repository first? | DigitalOcean](https://www.digitalocean.com/community/questions/how-to-copy-a-docker-image-from-one-server-to-another-without-pushing-it-to-a-repository-first)
## backlinks
- [[developer/developer_box📦\|developer_box📦]]