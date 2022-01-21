# Access controls
- is application of constraints on who(or what) can perfom attempted actions or access resources that they have requested.
- **Authentication** indentifies user and confirms they are who they say they are
- **Session Management** identifies which subsequent HTTP requests are being made by that same user
- **Access control** determines whether the user is allowed to carry out the action that they are attempting to perfom

#### Categories
1. Vertical access controls
2. Horizontal access controls
3. Context-dependent access controls (restrict access to functionality and resources based upon the state of the application or the user's interaction with it.)

### Examples of broken access controls
#### Vertical privilege escalation
1. Unprotected functionality
.e.g robots.txt, urls in client-side code ect
2. Parameter-based access controls methods
e.g. role = 1, admin = true etc
3. Platform misconfiguration
4. etc

#### Horizontal priviledge escalation
1. User ID controlled by request parameter
2. User ID controlled by request parameters, with unpredictable user IDS
3. User ID controlled by request parameter with data leakage in redirect
4. etc

#### Horizontal to vertical privilege escalation
1. User id controlled by request parameter with password disclosure
2. etc

#### Insecure direct object references
1. Insecure direct object references
2. etc

#### Access based vulnerabilities in multi-step processes
1. Multi step process with no access control on one step
2. etc

#### Referrer based access controls
1. Checking referer header before access to a resource or page

#### Location based access controls
- Using a vpn or proxy
- etc

### Preventing access control vulnerabilities
- Don't rely on obsufication alone for access control
- Unless a resource is intended to be publicly accessible, deny access by default
- Wherever possible, use a single application wide mechanism for enforcing access controls
- At code level, make it mandatory for developers to declare the access that is allowd for each resource, and deny access by default
- Thoroughly audit and test access controls to ensure they are working as designed.
