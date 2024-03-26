## Where to find?
http://wpdirectory.net

# XSS
Common input to look for:
- $\_GET
- $\_POST
- $\_REQUEST
- $\_SERVER\['REQUEST_URI'\]
- $\_SERVER\['PHP_SELF'\]
- $\_SERVER\['HTTP_REFERER'\]
- $\_COOKIE

These are also two functions that can lead to XSS vulnerabilities 
- add_query_arg()
- remove_quesry_arg()

These functions remove query from arguments from URLs.
They are insecure because they use $\_SERVER\['PHP_SELF'\] if the supplied second or third parameter is not a URL.


If you find xss in admin and it brings false positive which is occurs when unfiltered_html is enabled for users.
- Disable the unfiltered_html


# SQLi

Functions which are vulnerable to sqli 

- $wpdb->query()
- $wpdb->get_var()
- $wpdb->get_row()
- $wpdb->get_col()
- $wpdb->get_results()
- $wpdb->replace()

**Mitigation:**
Pass the query from 
$wpdb->prepare() function which mitiagates all the sqli attacks

**Safe Functions**
- $wpdb->insert()
- $wpdb->update()
- $wpdb->delete()

How developers fools?
- esc_sql()
- escape()
- esc_like()
- like_escape()

You can bypass them after understanding the logic as they only escape values in string as if you pass them without quote then you can bypass them.

You can use **SQLmap** for the exploitation.


# RCE
- eval()
- assert()
- preg_replace()
- php://input
- call_user_func()

Check for this one command injection.

# PHP Object Injection
- unserialize()
- maybe_unserialize()

If there is not any sanitization of piror user input then it is a PHP object injection.

**Tools to find:**

Burp extension:
https://gist.github.com/ethicalhack3r/7c2618e5fffd564e2734e281c86a2c9b

Wordpress plugin:
https://gist.github.com/ethicalhack3r/7c2618e5fffd564e2734e281c86a2c9b


# Privilege Escalation
User supplied input passed to these function.
- update_option()
- do_action()

Report:
https://www.wordfence.com/blog/2018/11/privilege-escalation-flaw-in-wp-gdpr-compliance-plugin-exploited-in-the-wild/

# File inclusion
Uvalid userinput pass
- include()
- require()
- include_once()
- require_once()
- fread()


# File Upload
==The sanitize_file_name() func will let bypass file uplaod restriction by `test.(php)` which will return test.php==
- sanitize_file_name()



# File Download
improper validation leads to arbitrary file read.

- file()
- readfile()
- file_get_contents()

# Open Redirection
- wp_redirect()

This function doesnt validate hosts.
**Mitigation:**
- wp_safe_redirection()

# SSL/TLS Issues
- CURLOPT_SSL_VERIFYHOST if set to 0 then does not
check name in the host certificate.
- CURLOPT_SSL_VERIFYPEER if set to FALSE then does
not check if the certificate (inc chain), is trusted. A Man-inthe-
Middle (MitM) attacker could use a self-signed
certificate.
- Check if plaintext HTTP is used to communicate with
backend servers or APIs. A grep for "http://" should be
sufficient.

Refrences:
how to find wordpress vunlerabilities wpscan ebook.pdf