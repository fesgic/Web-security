## Cross site Request Forgery(CSRF)
- is a web sec vulnerability that allows an attacker to induce users to perfom actions that they do not intend to perfom.
- allows an attacker to partly circumvent the same origin policy which is designed to prevent websites from interfering with each other.

## Impact
- causes victim to carry out an action unintentionally.e.g. change email address on their account etc
- depending on nature of attack, attacker might be able to gain full control over the user's account

## How CSRF works
- 3 conditions need to be in place:
1. Relevant action
action attacker has reason to induce.e.g -modifying permissions for other users/changing user's own pass
2. Cookie based session handling
-perfoming action involves issuing one or more HTTP requests and application solely relies on session cookies to identify the user who has made the requests.(no other meachanism in place for tracking sessions or validating user requests)
3. No upredicatable request parameters
- requests that perfom action do not contain any parameters that attacker cannot determine or guess.e.g. whn causing a user to change their password, the function is not vulnerable if attacker needs to know value of existing password.
e.g.
```
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Cookie: session=yvthwsztyeQkAPzeQ5gHgTvlyxHfsAfE

email=wiener@normal-user.com
```
- this meets the condition for CSRF.
- with this, attacker can construct a web page containing the HTML
```
<html>
    <body>
        <form action="https://vulnerable-website.com/email/change" method="POST">
            <input type="hidden" name="email" value="pwned@evil-user.net" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```
- If victim visits attackers page:
- attacker page triggers http request to vulnerable site
- user is logged in to vulnerable web site, their browser automaticallt includes their session cookie in the request(assumes samesite cookies aren't being used).
- vulnerable website will process the request the normal way, treating it as having being made by the victim user and change their email address.

## Preventing CSRF attacks
- The token should be:
1. Unpredictable with high entropy
2. Tied to user's session
3. Strictly validated in every case before any action executed.
4. Using same site cookies

## Common CSRF Vulnerabilities
1. Validation of CSRF token depends on request method.
2. Validation of CSRF token depends on token being present
3. CSRF token is not tied to the user session.
4. CSRF token is tied to a non-cookie session
- some apps do not tie the csrf token to session cookie .e.g 2 different frameworks for session and csrf cookie handling and not integrated together.
5. CSRF tokein is simply duplicated in a cookie