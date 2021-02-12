# Mapping the application

The first step of attacking a web app is discovering your attack surface

# Enumerating Content and functionality

- click every link and navigate to every page

Web spidering
- These tools request a page, get the links on that page, and repeat recursivley
- They can also manually submit HTML forms with preset or random values
- some parse javascript in an attempt to find more urls
- Example tools: Burp suite, WebScarab, Zed attack proxy, and CAT
- always check robots.txt
- Automated limitations:
- -> cant do unusual naviagtion mechanisms, eg dynamic menus made with js)
- -> links burried in compiled client-side objects, eg flash or java applets
- -> fields that require specific details will not work with random values eg, an email box may need @ and a valid email provider.
- -> spiders do not follow links they have already visited, but submitting a form may make a new url each time to show a different response, this causes infinite loop
- -> The spider must have access to a token in order to visit most of the site. Auth sessions can be break for reasons:
- --> at some point the spider will do a get request for logging out
- --> the program may see this spider as malicious and terminate it's session
- --> if the site uses per-page tokens, the spider will likely fail
- spiders can cause massive damage, deleting accounts, dropping databases, etc.

User-Directed Spidering
- Manually navigate the site, but the proxy stores the requests and responses for later
- benefits:
- -> the user knows how to follow unusual navigation mechanisms
- -> the user controls data input, and can give correct info
- -> the user can log in normally and let the browser manage the tokens
- -> dangerous links can be avoided 

Discovering Hidden Content
- Things spidering wont find but is usefull:
- -> Backup copies of live files, can leak source code
- -> backup archives containing sitemaps, code snapshots etc
- -> New functionality that has been deployed on the server but not linked to the public
- -> Default application functionality
- -> Old versions of files
- -> configs, could contain sensitive data
- -> Source files
- -> Comments in source code talking to devs, eg "remove this func after done" 
- -> Log files, could contain senstive info, usernames, session tokens, URLs etc

Brute-Force techniques
- Do **not** assume 200 means found and 404 means not found
- 302 Found, if redirects to a login page it means that the content needs an account access
- 401 / 403 - cannot be accessed by your user
- 500 Internal Server Error, during content discovery usually indicates certain parameters we're not submitted

Inference from Published Content
- most apps have a naming scheme, by following their scheme and adapting your wordlist, you will find more
- look at how the devs abreviate words or phrases and use that style

Use of Public Information
- Links may have existed in the past, but not now. 
- To find these you can use:
- -> Search engines
- -> waybackmachine
- Look for third parties that interact with your target
- Compile a list of devs, their emails and google dork for them on sites like github or stack overflow.

Leveraging the web server
- vulns exist that allow you to map the web server itself
- default files, and CMS may lead to mapping and old vulns

Discovering Hidden Parameters
- debug=true
- other common debug paramter names, debug, test, hide, source, etc. and common values, true, yes, on, 1, etc.

# Analyzing the application
