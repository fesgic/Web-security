## XSS.md
- allows an attacker to compromise the interactions that users have with a vulnerable application.
- allows an attacker to circumvent the same origin policy, which is designed to segregate different websites from each other.
- xss(Cross Site Scripting) allows attacker to masquarade as a victim user to carry out any actions that the user is able to perform and to access any of the user data.

## How does XSS work?
- xss works by manipulating a vul web so it returns malicious js to users. when it executes inside a victims browser, the attacker can fully compromise their interaction with the application.

## Proof of Concept
- can be done by  injecting a payload that causes your own browser to execute some arbitrary JavaScript.
e.g. print();

## Types of xss
1. [Reflected XSS](./ReflectedXSS.md)
- Where malicious script comes from current HTTP request
2. [Stored XSS](./StoredXSS.md)
- Where malicious script comes from the website's database
3. [DOM-based XSS](./DomBasedXSS.md)
- Where vulnerability exists in client side code rather than server-side code.
4. [XSS Contexts](./XSSContexts.md)
5. [Exploiting XSS](./ExploitingXSS.md)