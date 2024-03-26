-   Identify file upload function
-   Perform a normal file upload using an authenticated user (if possible)
-   Send the request to burp comparer
-   Remove the cookie or session identifier from the request
-   View the response to assess if file upload is possible without authentication

1. If within testing scope, assess if DoS is possible via file upload or disk filling from a single session. Use a low number (~100) of jpg files and use Burp intruder Number payload option to increment the payload names, e.g. image1.jpg, image2.jg, image4.jpg etc.
2. Upload a large file and assess if the application allows the upload.

If shell access is available on the test, it’s easier to perform a server side assessment checking for `LimitRequestBody` within the Apache config and `MAX_FILE_SIZE` within php.ini