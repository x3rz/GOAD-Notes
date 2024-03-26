BurpSuite

BurpSuite

    Proxy - What allows us to funnel traffic through Burp Suite for further analysis
    Target - How we set the scope of our project. We can also use this to effectively create a site map of the application we are testing.
    Intruder - Incredibly powerful tool for everything from field fuzzing to credential stuffing and more
    Repeater - Allows us to 'repeat' requests that have previously been made with or without modification. Often used in a precursor step to fuzzing with the aforementioned Intruder
    Sequencer - Analyzes the 'randomness' present in parts of the web app which are intended to be unpredictable. This is commonly used for testing session cookies
    Decoder - As the name suggests, Decoder is a tool that allows us to perform various transforms on pieces of data. These transforms vary from decoding/encoding to various bases or URL encoding.
    Comparer - Comparer as you might have guessed is a tool we can use to compare different responses or other pieces of data such as site maps or proxy histories (awesome for access control issue testing). This is very similar to the Linux tool diff.
    Extender - Similar to adding mods to a game like Minecraft, Extender allows us to add components such as tool integrations, additional scan definitions, and more!
    Scanner - Automated web vulnerability scanner that can highlight areas of the application for further manual investigation or possible exploitation with another section of Burp. This feature, while not in the community edition of Burp Suite, is still a key facet of performing a web application test.
	
	

Pitchfork -  allows us to select multiple payload sets (one per position) and iterate through them simultaneously

Battering ram - allows us to use one payload set in every single position we've selected simultaneously

cluster bomb - 	allows us to select multiple payload sets (one per position) and iterate through all possible combinations

sniper -  allows us to cycle through our payload set, putting the next available payload in each position in turn

# Extensions

   **Logger++** - Adds enhanced logging to all requests and responses from all Burp Suite tools, enable this one before you need it ;)
    **Request Smuggler** - A relatively new extension, this allows you to attempt to smuggle requests to backend servers. See this talk by James Kettle for more details: Link
    **Autorize** - Useful for authentication testing in web app tests. These tests typically revolve around navigating to restricted pages or issuing restricted GET requests with the session cookies of low-privileged users
    **Burp Teams Server** - Allows for collaboration on a Burp project amongst team members. Project details are shared in a chatroom-like format
    **Retire.js** - Adds scanner checks for outdated JavaScript libraries that contain vulnerabilities, this is a premium extension
    **J2EEScan** - Adds scanner test coverage for J2EE (java platform for web development) applications, this is a premium extension
    **Request Timer** - Captures response times for requests made by all Burp tools, useful for discovering timing attack vectors 
	**Bookmark** - Allows you to bookmark the requests.
 
 
 
 
 
 **What can we load into Comparer to see differences in what various user roles can access? This is very useful to check for access control issues.**

**Ans: sitemaps**

