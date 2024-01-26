# htpasswd and Admin portals

### Breach description:

Looking into the /whatever directory is a file called htpasswd  that contains the following information:

```jsx
root:437394baff5aa33daa618be47b75cb49
```

After decoding the weak MD5 we find that the credentials are root:qwerty123@, these can be accessed to login to the admin page located at /admin

**d19b4823e0d5600ceed56d5e896ef328d7a2b9e7ac7e80f4fcdb9b10bcb3e7ff**

### Breach Explanation:

A common practice when looking for vulnerabilities is to look for open ports on a server with a tool like nmap. In this case a simple 

```jsx
nmap --top-ports 1000 -sV 192.168.64.4

Starting Nmap 7.93 ( https://nmap.org ) at 2024-01-26 12:25 CET
Nmap scan report for 192.168.64.4
Host is up (0.0029s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.4.6 (Ubuntu)
|_http-server-header: nginx/1.4.6 (Ubuntu)
| http-robots.txt: 2 disallowed entries 
|_/whatever /.hidden
|_http-title: BornToSec - Web Section
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 48.79 seconds
```

Shows no open ports beyond 80 (damnit!)

But it does tell us some information about the server, and the name of two entries in the ttp-robots.txt, this tells we crawlers not to index these directories, but simultaneously tell us that they exist. After looking into the /whatever directory, we find the credential file.

after trying to use these credentials on the regular login page without success, we assume they could be used for some sort of server administration portal. Trying a few obvious options, we quickly fall on /admin, where the credentials lead to the flag.

### Breach Fix:

This one is a bit odd, probably not putting a credentials file in an easily accessible and (indirectly) referenced location would be a good start. Stronger encryption, and even a better password to start with would have both helped as well.
