If the application has an export function assess if it’s possible to injection macros within the web application that could be executed client side by another user.

Example:

-   Attacker injects malicious payload within the web application
-   Administrator logs in and exports the web application data to CSV
-   Attackers injected payload is then executed client side by the victims Excel, it’s likely even if excel prompts or warns the victim will proceed as the exported data is from a site they trust.

**Key Points:**

1.  Upload a malicious payload and access if it’s possible to download the payload from a back end interface.
2.  Verify the payload is the same file using a checksum