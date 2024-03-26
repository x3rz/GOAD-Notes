**Finding possible vulnerabilities with parameter names**

- Go to `grep.app`
- Choose a paramter example `?q=` mostly used when there a search query basically and XSS is common in that place.
- For finding XSS enter a query `echo $_GET['q`
- This will show all the results with the paramter `?q*=` includes: query and q.

<center> This is how you hunt for XSS with the parametes </center>

## Tasks
- Make a tool to check utilize gf in source code.
- Search more parameters.
- 