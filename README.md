# Reverse engineering / Security basic checks

- [ ]  A1 - Injections
    - [ ]  Check that you are safe from SQL injection
        - [ ]  Check that you use parametrization for queries at the  backend
            - [ ]  Try to insert SQL statements like `value'; DROP database db_...`
            - [ ]  Check that you don't have any SQL injection through GET (example `...?...username=toto' or 1=1%23&....`)
    - [ ]  Check if no string is executed remotely
        - [ ]  Avoid "python-os.system" like commands
        - [ ]  All inputs are filtered (regex, verifications, etc.) both in front-end and backend
    - [ ]  Check if you are safe from Local File Inclusion and Remote File Inclusion attacks
        - [ ]  No dynamic file is included at run time or at least this inclusion is safe (when server side rendering) (optional)

- [ ]  A2 - Broken authentication
    - [ ]  Check if no sensitive data is provided at login, signup, reset-password, etc..., *e.g.* Invalide username, Unknown email, etc...
    - [ ]  Rate-limit authentication failures (// sessions)
    - [ ]  Log unsuccessful authentications
    - [ ]  Check if ReCaptcha is implemented for login, signup, etc...

- [ ]  A3 - Sensitive Data Exposure
    - [ ]  Check that no sensitive route is publicly explosed
    - [ ]  Check if any other website is hosted in the same server with [robtex](https://www.robtex.com/), and more specifically in the *REVERSE* section.
    - [ ]  Check if no sensitive subdomain is exposed, *e.g.* [admin.website.com](http://admin.website.com), [preprod.website.com](http://preprod.website.com), [ftp.website.com](http://ftp.website.com) ...
        - [ ]  You may use [knock](https://github.com/guelfoweb/knock), [pentest-tools](https://pentest-tools.com/), [Sublist3r](https://github.com/aboul3la/Sublist3r)
    - [ ]  Check if no hidden file is served
        - [ ]  dirb https://mytargetwebsite.com
    - [ ]  Store hashed passwords in database (bcrypt)
    - [ ]  The public IP doesn't expose other ports than 80 and 443 (and 22) (can be verified with `nmap -sV` or `nmap -sT`)

- [ ]  A4- XXE

- [ ]  A5 - Broken Access Control
    - [ ]  Check the extensive usage of JWT to permit access to protected data for users
    - [ ]  Check if no confidential route is exposed for public or authenticated users (GET, POST, PUT, DELETE, etc.)
        - [ ]  JWT usage to avoid unwanted data access. What is your JWT management policy ?
        - [ ]  Check that you can't access to forbidden data with the same cookie
        - [ ]  Check that you cannot access to data through manipulating routes (example GET document/1, document/2 ...)
        - [ ]  Check cookies scope and security settings : Domain, Path, HttpOnly, Secure, etc.
    - [ ]  Invalidate JWT on server side
    - [ ]  No REACT_APP_* variable containing API keys can be found at Sources / Debogger tab
    - [ ]  No API key can be found with a basic search
    - [ ]  Minimize CORS usage

- [ ]  A6 - Security misconfiguration
    - [ ]  Firefox/Chrome Devtools
        - [ ]  No logs in the console when making a complete route
        - [ ]  No sensitive data related to the backend returned back by API call
        - [ ]  Website source obfuscation : `"build": "GENERATE_SOURCEMAP=false ....... build"`
        - [ ]  All tokens in cookies, HttpOnly ...
        - [ ]  No sensitive data in local storage / session storage / Cache storage / Indexed BD
    - [ ]  Files upload
        - [ ]  Check if any antimalware is implemented and integrated in upload process
        - [ ]  Check max size
        - [ ]  Verify both mime-type and file extension
        - [ ]  Check if remote directory only has w+r rights
        - [ ]  Check if we recreate files (optional) ...
        - [ ]  Check that ids given to documents for download are not sequential
    - [ ]  Check if https is implemented and how https renewal is handled
        - [ ]  Is https correctly configured ? : use [https://www.ssllabs.com](https://www.ssllabs.com/)
        - [ ]  Redirection http → https
        - [ ]  No insecure elements included in the web app (insecure scripts/iframes/media contents, etc.)
        - [ ]  Check if you have added Strict-Transport Security Policy to your web server, *e.g.* NGINX : `add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;` under `listen 443 ssl;`
        - [ ]  Expiracy alerts ?
    - [ ]  Check that you are safe from CSRF attacks
    - [ ]  Check if you are resilient to DDoS attack

- [ ]  A7 - Cross Site Scripting - XSS
    - [ ]  Check if you can inject `<script>alert('test')</script>` if you inject html/javascript in your web application
        - [ ]  Solution : escape inputs before inserting them is html

- [ ]  A8 - Insecure Deserialization
    - [ ]  Check that you are using a library to perform deserialization : jackson, etc...

- [ ]  A9 - Using components with known vulnerabilities
    - [ ]  Check if any sensitive data like *versions* of the technologies in use with the following tools [BuiltWith](https://chrome.google.com/webstore/detail/builtwith-technology-prof/dapjbgnjinbpoindlpdmhochffioedbn?utm_source=chrome-ntp-icon), [Wappalyzer](https://chrome.google.com/webstore/detail/wappalyzer/gppongmhjkpfnbhagpmjfkannfbllamg?utm_source=chrome-ntp-icon) and [Whatruns](https://chrome.google.com/webstore/detail/whatruns/cmkdbmfndkfgebldhnkbfhlneefdaaip?utm_source=chrome-ntp-icon) extension (also available for firefox). Could we hide as much data as we can ?
        - [ ]  Check if any known vulnerability reported at [https://www.exploit-db.com/](https://www.exploit-db.com/), [https://snyk.io/](https://snyk.io/) if we can't hide technologies versions.
    - [ ]  Check that you don't have critic breaches in OSS components :
        - [ ]  `mvn org.sonatype.ossindex.maven:ossindex-maven-plugin:audit -f pom.xml`
        - [ ]  `yarn audit`

- [ ]  A10 - Insufficient Logging & Monitoring
    - [ ]  Which logging software do you use ? We recommend Sentry ...
