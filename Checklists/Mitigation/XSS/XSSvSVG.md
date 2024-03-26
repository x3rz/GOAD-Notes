## XSS via SVG
-   Direct view - ==vulnerable== - The file is linked to directly.
-   Direct view with content-disposition: attachment - ==not vulnerable== - Headers are sent to force the file to be downloaded.
-   Direct view with CSP - ==not vulnerable== - The Content Security Policy is set to disallow inline JavaScript.
-   Image Tags - ==not vulnerable== - The SVG is referenced through image tags which prevent scripts.
-   Tags With CSP - ==not vulnerable== - Image tags and the same CSP as above for double protection.
-   Sanitised through Inkscape - ==vulnerable== - This is a direct view but the file has been processed by the following command:  
    
    ```
    inkscape --file="xss.svg" --verb="FileVacuum" --export-plain-svg="sanitised.svg"
    ```
    
    It was expected that this would remove the JavaScript but it did not.
-   Image in an iframe - ==vulnerable== - The SVG is loaded as the source for the iframe with no special attributes set.
-   Image in a sandboxed iframe - ==not vulnerable== - The SVG is loaded as the source for the iframe but the [sandbox](https://www.w3schools.com/tags/att_iframe_sandbox.asp) attribute is set to block scripts.

