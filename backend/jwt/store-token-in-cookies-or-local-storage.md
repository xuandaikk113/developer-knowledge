# LocalStorage vs Cookies: All You Need To Know About Storing JWT Tokens Securely in The Front-End
## 1. Local Storage
### Pros: It's convenient.

- It's pure JavaScript and it's convenient. If you don't have a back-end and you're relying on a third-party API, you can't always ask them to set a specific cookie for your site.
- Works with APIs that require you to put your access token in the header like this: Authorization Bearer ${access_token}.

### Cons: It's vulnerable to XSS attacks.

- An XSS attack happens when an attacker can run JavaScript on your website. This means that the attacker can just take the access token that you stored in your localStorage.
- An XSS attack can happen from a third-party JavaScript code included in your website, like React, Vue, jQuery, Google Analytics, etc. It's almost impossible not to include any third-party libraries in your site.

## 2. Cookies
### Pros: The cookie is not accessible via JavaScript; hence, it is not as vulnerable to XSS attacks as localStorage.

- If you're using httpOnly and secure cookies, that means your cookies cannot be accessed using JavaScript. This means, even if an attacker can run JS on your site, they can't read your access token from the cookie.
- It's automatically sent in every HTTP request to your server.

### Cons: Depending on the use case, you might not be able to store your tokens in the cookies.

- Cookies have a size limit of 4KB. Therefore, if you're using a big JWT Token, storing in the cookie is not an option.
- There are scenarios where you can't share cookies with your API server or the API requires you to put the access token in the Authorization header. In this case, you won't be able to use cookies to store your tokens.

## 3. About XSS Attack
Local storage is vulnerable because it's easily accessible using JavaScript and an attacker can retrieve your access token and use it later. However, while httpOnly cookies are not accessible using JavaScript, this doesn't mean that by using cookies, you are safe from XSS attacks involving your access token.

If an attacker can run JavaScript in your application, then they can just send an HTTP request to your server and that will automatically include your cookies. It's just less convenient for the attacker because they can't read the content of the token although they rarely have to. It might also be more advantageous for the attacker to attack using victim's browser (by just sending that HTTP Request) rather than using the attacker's machine.

## 4. Cookies and CSRF Attack
CSRF Attack is an attack that forces a user to do an unintended request. For example, if a website is accepting an email change request via:
```
POST /email/change HTTP/1.1
Host: site.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 50
Cookie: session=abcdefghijklmnopqrstu

email=myemail.example.com 
```
Then an attacker can easily make a form in a malicious website that sends a POST request to https://site.com/email/change with a hidden email field and the session cookie will automatically be included.

However, this can be mitigated easily using sameSite flag in your cookie and by including an anti-CSRF token.

## 5. Conclusion
Although cookies still have some vulnerabilities, it's preferable compared to localStorage whenever possible. Why?

- Both localStorage and cookies are vulnerable to XSS attacks but it's harder for the attacker to do the attack when you're using httpOnly cookies.
- Cookies are vulnerable to CSRF attacks but it can be mitigated using sameSite flag and anti-CSRF tokens.
- You can still make it work even if you need to use the Authorization: Bearer header or if your JWT is larger than 4KB. This is also consistent with the recommendation from the OWASP community:
    
    `Do not store session identifiers in local storage as the data are always accessible by JavaScript. Cookies can mitigate this risk using the httpOnly flag.`