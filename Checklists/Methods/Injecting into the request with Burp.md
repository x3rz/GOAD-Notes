1.  Upload a legitimate image using burp and verify upload is successful
2.  Send the previous request to burp repeater
3.  After the legitimate image data in the request, attempt to inject payloads (injection payloads or a reverse shell)
4.  Submit the request
5.  Download the uploaded file from the target server, verify it has the contained payload within, can you leverage this to execute a payload on the target server?

![burp php shell injection in an image](https://www.aptive.co.uk/wp-content/uploads/2016/11/burp-php-shell-injection-in-an-image-1.png)

---

#### Verify the file is Uploaded

Regardless of the method you used in the previous step (EXIF or Modifying the Burp Request) create a checksum of the file locally and upload to the target, download the file from the target and verify the checksum matches.


1.  Checksum a file, upload it to the app, download it from the app, verify it’s the same file