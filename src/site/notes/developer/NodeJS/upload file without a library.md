---
{"dg-publish":true,"permalink":"/developer/NodeJS/upload file without a library/","dgPassFrontmatter":true}
---

I like to remove any package dependancies where I can. Hopefully this solution can help remove another

```js
var fs = require('fs');
var request = require('request');
var upfile = 'sample.zip';
fs.readFile(upfile, function(err, content){
    if(err){
        console.error(err);
    }
    var metadata = {
        token: "### access token ###",
        channels: "sample",
        filename: "samplefilename",
        title: "sampletitle",
    };
    var url = "https://slack.com/api/files.upload";
    var boundary = "xxxxxxxxxx";
    var data = "";
    for(var i in metadata) {
        if ({}.hasOwnProperty.call(metadata, i)) {
            data += "--" + boundary + "\r\n";
            data += "Content-Disposition: form-data; name=\"" + i + "\"; \r\n\r\n" + metadata[i] + "\r\n";
        }
    };
    data += "--" + boundary + "\r\n";
    data += "Content-Disposition: form-data; name=\"file\"; filename=\"" + upfile + "\"\r\n";
    data += "Content-Type:application/octet-stream\r\n\r\n";
    var payload = Buffer.concat([
            Buffer.from(data, "utf8"),
            new Buffer(content, 'binary'),
            Buffer.from("\r\n--" + boundary + "--\r\n", "utf8"),
    ]);
    var options = {
        method: 'post',
        url: url,
        headers: {"Content-Type": "multipart/form-data; boundary=" + boundary},
        body: payload,
    };
    request(options, function(error, response, body) {
        console.log(body);
    });
});
```
## Credits
- [node.js - NodeJS Request how to send multipart/form-data POST request - Stack Overflow](https://stackoverflow.com/questions/49053193/nodejs-request-how-to-send-multipart-form-data-post-request)
## Backlinks
- [[developer/NodeJS/NodeJS\|NodeJS]]