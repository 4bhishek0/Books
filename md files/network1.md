# DNS

DNS let user connect to the websites using domain names instead ip addresses.likes google.com,facebook.com.DNS translates domain names to ip addresses so browsers can load resources.

There are four DNS servers  involved in loading web pages :
1.DNS recursor -- it is a server designed to receive queries from client machines through web browsers.
2.Root nameserver-- It is the first step in translating human readable host names into  ip addresses.
3.TLD nameserver -- Top Level Domain server is the next step and it hosts the last name  of a hostname "com".
4.Authoritative nameserver -- It is the last stop in the nameserver query  it returns the requested IP Addresses to the DNS recursor that made the initial request.

Caching is a data persistence process that helps short-cicuit the necessary requests by serving the requested resource record earlier in the DNS Lookup.

## Web app security vulnerabilities

1.Cross site scripting(XSS) - It allows an attacker to inject malicious code  into the web pages to access important information .
2.SQL injection - It is a way through which attacker exploits vulnerabilties to gain access .
3.Dos - Denial of service attacks are able to overload server with different types of attack traffic makin the server to deny the requests from legitimate users.
4.Cross-site-request forgery(CSRF) -- It involves tricking a victim into making request that utlizes their authentication generallly involved attacks on admins

## HTTP

HTTP  is the foundation of the world wide web ,and is used to load web pages using hypertext links.
HTTP request includes different types of information :
HTTP version type
URl
HTTP method --GET or POST requests
HTTP request headers -- text information that includes information such as which browser is using and what data is being requested.
optional HTTP body

4xx -- client side Error ,,404 NOT FOUND
5xx -- server side error ,,



HTTPS (hyper test transfer protocal secure) is the secure version of HTTP,which is the primary protocol used to send data between a web browser and a website.(in browser websites that not use https shows them as NOT SECURE).IT uses encryption protocol to encrypt communications called TLS(Transport Layer Security), formerly now as SSL(Secure Socket Layer).Uses two different keys to encrypt data.

## Cookies

Cookies are small files of information that a web server generates and send to the web browser.helps inform websites about the user ,enabling the websites to personalize the user experience.
Session cokkies, tracking cookies , Authentication cookies,persistent cookies ,Zombie cookies
Third party cookies that belong to the domain not your cookies i.e, firstparty cookies.