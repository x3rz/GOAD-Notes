1.  Upload a file with the semi colon after the black listed extension, such as: `shell.asp;.jpg`
2.  Upload a directory with the .asp extension, then name the script within the directory with a permitted file extension, example: `folder.asp\file.txt`
3.  When serving PHP via IIS `< > and .` get converted back to `? * .`
4.  Use characters that can replace files, example `>>` can replace `web.config`
5.  Try using spaces or dots after characters, example: `foo.asp..... .. . . .`
6.  `file.asax:.jpg`
7.  Attempt to disclose information in an error message by uploading a file with forbidden characters within the filename such as: `| %< * ? "`


# Apache

1.  Identify what characters are being filtered – use burp intruder to assess the insert points with a meta character list
2.  Ensure your list contains uncommon file extension types such as `.php5`,`.php3`,`.phtml`
3.  Test for flaws in the protection mechanism, if it’s stripping file names can this be abused? Example: `shell.p.phpp` if the app strips .php it could rename the extension back to .php
4.  Try a null byte `%00` at various places within the file name, example: `shell.php%00.jpg`, `shell.php%0delete0.jpg` – observe how the application responds
5.  Double extensions: if the application is stripping or renaming the extension – What happens if you give it two extensions? Example: `shell.php.php` or 4 `shell.txt.jpg.png.asp`
6.  Try long file names, example `supermassivelongfileeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeename.php` apply other filter bypass techniques used in conjunction with long file names
7.  Try `test.asp\`, `test.asp.\`
8.  Can you upload the flash XSS payload, that is named as a .jpg
9.  Try the previous technique but use PDF or Silverlight instead
10.  Same again but attempt to abuse crossdomain.xml or clientaccesspolicy.xml files
11.  Try using encoding to bypass blacklist filters, try URL, HTML, Unicode and double encoding
12.  Combine all of the above bypass techniques
13.  Try using an alternative HTTP Verb, try using POST instead of PUT or GET (or vice versa), you can enumerate the options using Burp Intruder and the HTTP Verbs payload list
14.  Additionally, ensure all input points are fuzzed for various input validation failures such as, XSS, Command Injection, XPath, SQLi, LDAPi, SSJI