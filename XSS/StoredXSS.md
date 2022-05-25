## Stored XSS
- With stored XSS, the application instead stores the input and embeds it into a later response in an unsafe way.
e.g.
- a message board app lets user write message and display to other users:
```
<p>Hello, this is my message</p>
```
- attacker can craft a message:
```
script> /* Malicious code here */ </script>
```
- the comment is stored in the db and later when viewed, people using the board on retrieving the message would get:
```
<p><script> /* Malicious code here */ </script></p>
```
when they try to view the page with the malicious comment.

## Impact of stored XSS
- If an attacker can control a script that is executed in the victim's browser, then they can typically fully compromise that user.
- the attacker places their exploit into the application itself and simply waits for users to encounter it.
