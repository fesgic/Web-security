## DOM Based XSS
- arises when an application contains some client-side js that processes data from an untrusted source in an unsafe way, usually by writing the data back to the DOM.
- arise when JavaScript takes data from an attacker-controllable source, such as the URL, and passes it to a sink that supports dynamic code execution, such as eval() or innerHTML. This enables attackers to execute malicious JavaScript, which typically allows them to hijack other users' accounts.
- you need to place data into a source so that it is propagated to a sink and causes execution of arbitrary JavaScript.
- most common source for DOM XSS is the URL, which is typically accessed with the window.location object. 
An attacker can construct a link to send a victim to a vulnerable page with a payload in the query string and fragment portions of the URL. In certain circumstances, such as when targeting a 404 page or a website running PHP, the payload can also be placed in the path.
e.g.
- an app uses soms js to read value from input field and write that to an element within the HTML
```
var search = document.getElementById('search').value;
var results = document.getElementById('results');
results.innerHTML = 'You searched for: ' + search;
```
- if attacker can control input field, they can easily construct a malicious value that causes their own script to execute:
```
You searched for: <img src=1 onerror='/* Malicious code here... */'>
```
- In a typical case, the input field would be populated from part of the HTTP request, such as a URL query string parameter, allowing the attacker to deliver an attack using a malicious URL, in the same manner as reflected XSS.

## Testing for DOM Based XSS
1. Testing HTML sinks
2. Testing JS execution sinks
3. Testing using DOM Invader(inbuilt in burp).
