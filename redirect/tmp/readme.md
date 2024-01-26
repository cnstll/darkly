REDIRECT

# REDIRECT VULNERABILITY

### Breach description:

We can modify the redirect links like  

```jsx
index.php?page=redirect&site=someothersite
```

Gets us the flag

### Explanation of the breach:

The idea here is that we can craft legitimate looking links that redirect to phishing sites for example, the idea, for example, could be to send an email like:

```jsx
http://legitLink.com/index.php?page=redirect&site=lesslegitlink.onion
```

### Breach Fix

Not using dynamic redirects, especially when they offer very little value, as here.
