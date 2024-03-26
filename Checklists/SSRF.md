https://github.com/EdOverflow/bugbounty-cheatsheet/blob/master/cheatsheets/ssrf.md

https://github.com/cujanovic/SSRF-Testing

https://github.com/jdonsec/AllThingsSSRF

- [ ] Try to fetch image with `file://protocol` if you can then hurray

- [ ] If no payloads working then send
	- Host a ngrok server with script 
		`<?php header("Location: <http://127.0.0.1:443>");?>`
	- Send this `uniqID.ngrok.io/script.php` in `?url=` parameter.
	- Now wait for the resonse :P.
		- Ref: https://yasshk.medium.com/blind-ssrf-in-url-validator-93cbe7521c68

![](https://pbs.twimg.com/media/E6dXN25WEAA--U8?format=jpg&name=large)