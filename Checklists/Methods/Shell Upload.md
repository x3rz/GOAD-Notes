1.  Apache MIME Types: Attempt to upload a renamed file e.g. `shell.php.jpg` or `shell.asp;.jpg` and assess if the web server process the file by exploiting weak Apache MIME types
2.  Null Byte: Try a null byte `%00` at the end of the file name or within such as: `shell.php%0delete0.jpg`– observe how the application responds
3.  Can you upload dot files, if so can you upload a .htaccess file an abuse AddType: `AddType application/x-httpd-php .foo`
4.  Be mindful of any processing to upload files – Example: Could command injection be used within a file name that will later be processed by a backend backup script?
5.  Be mindful of any server side processing to upload files – If compressed files are permitted, does the application extract them or vice versa?
6.  Does the server Anti Virus process uploaded files? – Try uploading a compressed file type such as .zip, .rar etc if the server side AV is vulnerable, it’s possible to exploit and gain command execution.