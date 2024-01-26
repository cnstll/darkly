# XSS on feedback

### Breach description:

The name field in the feedback input does not sanitise user input, and can therefore be injected with some piece of script, that will then be executed when the page loads.

For some reason simply entering the work “script” inside the input reveals the flag.

**0FBB54BBF7D099713CA4BE297E1BC7DA0173D8B3C21C1811B916A3A86652724E**

### Breach Explanation:

Displaying input from other users is a dangerous game. A user could try to blur the line between the page’s HTML code and user input. If user input can be interpreted as HTML code, a malicious user will be able to use this to run scripts on other peoples browsers.

A working example in this case could be:

```jsx
<body onload="document.forms['guestform']['txtName'].value='Corrector'; document.forms['guestform']['mtxtMessage'].value='Best project, 200/100'; setTimeout(function() { document.forms['guestform']['btnSign'].click(); }, 3000);">
```

To do this we also have to bypass the trivial frontend limitations to max size of the input by editing it in the inspector for example

### Breach Fix:

The fix here would be to make sure that user input is only ever interpreted as a string, this could be through sanitation, escaping characters that could be useful to an attacker etc. Making an actual working size limitation would not, per say fix the issue but it would make the breach less easily exploitable.
