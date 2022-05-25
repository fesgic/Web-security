## XSS Contexts
1. XSS between html tags
- you need to introduce some new HTML tags desiged to trigger execution of js e.g.
```
<script>alert()</script>
<img scr=1 onerror=alert()>
```