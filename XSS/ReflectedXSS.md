## Reflected XSS
- Reflected XSS arises when an application takes some input from an HTTP request and embeds that input into the immediate response in an unsafe way.
example <br>
- a website url:
```
http://example.com/search?term=gift
```
and returns
```
<p> You searched for: gift</p>
```
- asssuming application doesn't perfom any other processing of the data, an attacker can construct an attack like:
```
http://example.com/search?term=<script>/* Malicious code or stuff here </script> 
```
and returns:
```
<p>You searched for: <script> /* Malicious code or stuff here */ </script>
```
- if another user of the application requests the attackers URL, then the script supplied by the attacker will execute in the victim user's browser, in the context of their session with the application.

## Impact
1. controlling a user's device after compromise
2. Perform any action within app that normal user can
3. view any info user is able to view
4. modify any info user is able to modify
5. initiate interactions with other application user's,.e.g malicious attacks that appear to be genuine from initial victim user.

## Different contexts of Reflected xss
- location of the reflected data within the application's response determines what type of payload is required to exploit it
- if the application performs any validation or other processing on the submitted data before it is reflected, this will generally affect what kind of XSS payload is needed

## Reflected XSS Contexts
1. XSS between html tags
- you need to introduce some new HTML tags desiged to trigger execution of js e.g.
```
<script>alert()</script>
<img scr=1 onerror=alert()>
```

## Find and test for reflected xss
1. Test every entry point
- Test separately every entry point for data within the application's HTTP requests.
2. Submit random alphanumeric values
- For each entry point, submit a unique random value and determine whether the value is reflected in the response.
- The value should be designed to survive most input validation, so needs to be fairly short and contain only alphanumeric characters. A random alphanumeric value of around 8 characters is normally ideal.
3. Determine the reflection context
- This might be in text between HTML tags, within a tag attribute which might be quoted, within a JavaScript string, etc.
4. Test a candidate payload
- Based on the context of the reflection, test an initial candidate XSS payload that will trigger JavaScript execution if it is reflected unmodified within the response.
5. Test alternative payloads
- If the candidate XSS payload was modified by the application, or blocked altogether, then you will need to test alternative payloads and techniques that might deliver a working XSS attack based on reflcection context
6. Test the attack in a browser
- transfer the attack to a real browser (by pasting the URL into the address bar) and see if injected js is executed. e.g. alert(document.domain).