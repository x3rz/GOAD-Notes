## Automate	
- [ ] XSScrapy
- [ ] XSSstrike
- [ ] XSS Gf patterns
- [ ] XSS wayback Hunt
- [ ] [[XSS via QR scanner]] 
- [ ] XSS via DNS entries
- [ ] kxss [[Automation]]
- [ ] [XSS via Anchor tag](https://drive.google.com/file/d/1gfDTFGsBKW73uRtmUM3D_4Qcb0B4Ez03/view)
- [ ] [[XSS in Json parsers,editors,writers]]
- [ ] Simple XSS?
	- [ ] [[XSS to Arbitrary file read]]
- [ ] Have X-XSS-Protection header?
	- [ ] Use firefox instead of chrome based web browser.
- [ ] [[XSS to OpenRedirection]]
- [ ] [[XSS to LFI]]
- [ ] [[XSS to Command exection]]
- [ ] htmlspecialchars
	- [ ] [Bypass by event handlers](https://github.com/X-Vector/XSS_Bypass/blob/master/htmlspecialchars%20-%20htmlentities/README.md)
- [ ] [Mardown parsers](https://github.com/cujanovic/Markdown-XSS-Payloads/blob/master/Markdown-XSS-Payloads.txt)
	- [ ] [Markdown parsers](https://github.com/JakobTheDev/information-security/blob/master/Payloads/md/XSS.md)
	- [ ] [[Markdown payloads]]
- [ ] Burp Collabrator for XSS
	- https://xsshunter.com/features
	- 
- [ ] Check [[High success Payloads]]
- [ ] Check [Lesseer known attrib](https://www.ptsecurity.com/ww-en/analytics/knowledge-base/what-is-a-cross-site-scripting-xss-attack/)
- [ ] [Check ](https://github.com/EdOverflow/bugbounty-cheatsheet/blob/master/cheatsheets/xss.md)

- payloads
	- [seclist](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/XSS/XSS-Cheat-Sheet-PortSwigger.txt)
	- [advance](https://html5sec.org/)
	- [payloadbox](https://github.com/payloadbox/xss-payload-list)
	- [XXS audiors(X-XSS-Protection)](https://www.netsparker.com/blog/web-security/xss-auditors/)
	- [Angular JS](https://spring.io/blog/2016/01/28/angularjs-escaping-the-expression-sandbox-for-xss)
	- [Angular JS Payloads](https://gist.github.com/mccabe615/cc92daaf368c9f5e15eda371728083a3)
	- [XSS Cheatsheet](https://paper.bobylive.com/Security/XSS_Cheat_Sheet_2018_Edition.pdf)


**Resource:**
[Payload creator](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

- [ ] If using Vue.js or angularjs then `{{alert(1)}}` try adding this [one](https://spring.io/blog/2016/01/28/angularjs-escaping-the-expression-sandbox-for-xss)
	- ``{{ this.constructor.constructor('alert("foo")')() }}``
	- ``{{$on.constructor('alert(1)')()}}``
	-  `{{ this.constructor.constructor('alert("foo")')() }}`
	-  `{{$on.constructor('alert(document.domain)')()}}`