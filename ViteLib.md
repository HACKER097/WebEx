## Login page

Man see login page, man try Sqlmap

![image](https://github.com/HACKER097/WebEx/assets/38581702/fa95d04d-0a57-4808-8658-915632f08c80)

`sqlmap -u "http://127.0.0.1:1337/api/login" --data="username=1&password=2"`

Sqlmap does not work because of a rate limit

## Content Security Policy

Provided hint is an (article)[https://book.hacktricks.xyz/pentesting-web/content-security-policy-csp-bypass] about bypassing Content Security Policy.

Its a way of preventing XXS, but there are some unsafe CSPs. This would mean I have to find which type of unsafe CSP this challange has, and exploit it.

Here is the CSP: `Content-Security-Policy default-src 'self' openlibrary.org;img-src 'self' raw.githubusercontent.com external-content.duckduckgo.com;base-uri 'self';font-src 'self' https: data:;form-action 'self';frame-ancestors 'self';object-src 'none';style-src 'self' https: 'unsafe-inline`

`'unsafe-inline` is supposed to be unsafe, lets see how it can be exploited.

Ok, second (article)[https://book.hacktricks.xyz/pentesting-web/content-security-policy-csp-bypass/csp-bypass-self-+-unsafe-inline-with-iframes]

Looks a but complicated, will come back to it.

## Links!

Clicking on the images for the books takes us to the page for that book

![image](https://github.com/HACKER097/WebEx/assets/38581702/31c39621-a76f-473f-9172-978d3a77916c)


But no link option is avalable when trying to add a book

![image](https://github.com/HACKER097/WebEx/assets/38581702/6c7886bd-fbbd-4074-a3ba-a94e484a4c95)


Looking at the book creation API request, it is an option! Just not part of the UI

![image](https://github.com/HACKER097/WebEx/assets/38581702/1798a084-1c54-4216-af20-1261e2c41006)

Lets see what happens when we link to a sussy link.


