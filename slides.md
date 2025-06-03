---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Cyber Security 

Full-Stack Development - part 7/8

- [ ] CyberSecurity Importance
- [ ] Cookies
- [ ] Basic Principles, Attack Vectors

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->


---
transition: slide-left
---

# Web Security
What's one thing you'd be terrified of if your web app got hacked?

- Definition: When the web server and all of its applications are protected and measures are put in place to protect it from harm
- Online threats are real (see Break slide)
- Users need to be able to trust that websites will keep their information safe (ex: credit card info, personal info, etc.)
- We need to be aware of the risks / pitfalls
- Consequences range from:
  - Loss of control, Loss of access, Identity theft, Data theft, Monetary theft, Spying, Embarassment

---
transition: slide-left
---

# BUT nothing can be 100% secure
M.I. [classic scene](https://www.youtube.com/watch?v=ifwngc8FHZM)

- new vulnerabilities arise as software changes or don't expect over time
  - software gets upgraded/added
  - configurations get changed
  - data gets updated
- Zero-Day Vulnerability: A software flaw which exposes a vulnerability but does not yet have a fix
- Overall security is only as strong as your weakest link
- Your mission: to raise the weakest points as difficult to exploit as possible, without making it a horrible experience for your users

---
transition: slide-left
---

# Principle of Least Privilege
Control access to systems and resources by granting as little access as possible

- Every program and every user should operate using the least amount of privilege necessary to complete the job
- ex: if user's job is to edit text, then they should not able to browse financial data
- ex: no user should be able to edit their system access privileges
- this applies to:
  - APIs, system resources, database access, software version control, webpages

---
transition: slide-left
---

# Other General Principles

1. Simple is more secure: A house with 1 door vs. ten houses with doors and windows?
   - The larger and more complex the system, the larger attack surface, the harder it is to secure
   - 💡 Use clearly named fns and vars, write comments answering 'why', prefer built-in well-tested libraries
2. Never trust users: be paranoid, every user is a potential hacker or victim of [social engineering](https://www.youtube.com/watch?v=oG5vsPJ5Tos)
   - devices/passwords can get stolen, user forgot to logout, disgruntled employee, contractors
   - 💡 ability to quickly revoke/quarantine access
3. Expect the unexpected
   - user enters no text, too much text, paste formatted text, emojis, HTML/JS, can db query be modified
   - 💡 purify/sanitize inputted text in case users submit accidental/intentional HTML/JS
4. Resilience: identify, respond, recover
   - maintain operational continuity
   - 💡 monitoring systems that notify if something go wrong/suspicious, DevOps team or on call staff
5. Security through obscurity: less information given out, the better
   - 💡 Hide info in login error messages, server info in response HEADER, web url file extensions

---
transition: slide-left
---

# Cookies (pg.1)

- since HTTP is stateless, cookies are used to:
   - keep track of currently authenticated user
   - store contents of a shopping cart
- stored as a header `Cookie: theme=dark`
   - Goto any website > Inspect > Applications > Cookies > click on website > identify a cookie, its value, and expiration
   - Go to console and type: `document.cookie` > what does it output?
  ```js
  document.cookie = "studentName=Albert"
  document.cookie = "session-id=abc123"
  ```
- Can you replace `studentName` with another value?


---
transition: slide-left
---

# Cookies (pg.2)

- `httpOnly` attribute - setting this to `true` prevents JS from accessing/modifying it
- `secure` attribute - ensures cookie is only sent over https connections 
   - won't send over http
   - prevents man-in-the-middle attacks
  ```js
  res.cookie('username, username, {
    httpOnly: true, // document.cookie will now be undefined
    secure: process.env.NODE_ENV === 'production'
  });
  ```
- Can you delete entire cookie on the client?  (No, but you can set expiration date to the past)
- If you don't set expiration, will default to duration of a `session` 
   ```js
   res.cookie('theme', 'dark', {
    // expires in 1 day
    expires: new Date(Date.now() + 86400000) // OR maxAge: 3600000 expires 1 hour
   });
   ```
---
transition: slide-left
---

# Heartland Data Breach 2009
Tthe company's computers were compromised, and 100 million debit/credit cards were stolen

- [Article 1](https://www.twingate.com/blog/tips/Heartland%20Payment%20Systems-data-breach), [Article 2](https://www.secureworks.com/blog/general-pci-compliance-data-security-case-study-heartland)
- SQL / NoSQL Injection: When apps pass raw user input into queries, attackers can inject operators/characters that modify the db query
  ```js
  // Insecure: directly using user input
  User.findOne({ username: req.body.username, password: req.body.password });

  // Attack Payload
  { "username": { "$ne": null }, "password": { "$ne": null }}
  ```
- Fixes:
   ```js
    // Validation + parameterization
    const { username, password } = req.body;
    if (typeof username !== 'string' || typeof password !== 'string') return res.status(400).send('Invalid input');
    User.findOne({ username, password });
   ```
- use bcrypt and token-based auth, express-validator or manual checks, sanitize via validator.js
- see example of [mySQL injection](https://www.youtube.com/watch?v=2OPVViV-GQk&t=128s)

---
transition: slide-left
---

# The Samy Worm Breach [🎧](https://darknetdiaries.com/transcript/61/)
Tthe company's computers were compromised, and 100 million debit/credit cards were stolen

- [Article](https://www.linkedin.com/pulse/how-cross-site-scripting-xss-took-down-myspace-tim-queen--0drke/)
- XSS: Cross Site Scripting can inject javascript when websites output raw HTML without escaping

1. In our foodtruck app, add a new foodtruck with title as `<script>alert('hacked')</script>`
2. In home.ejs, change the truck name output from `<%=` to `<%-`
   - But how did it get there in the first place? 💡 We should purify/sanitize our inputs
3. `npm i sanitize-html` 
  ```js
  const createTruck = async (req, res) => {
    const truckData = req.body;
    truckData.name = sanitize(truckData.name, {
      allowedTags: [],
      allowedAttributes: {},
    });
  ```
4. use [DOMPurify](https://cure53.de/purify) on your frontend
  ```js
    <img src="/icons/map-pin.svg" alt=""> ${DOMPurify.sanitize(temp, { ALLOWED_TAGS: [] })}</a>
  ```

---
transition: slide-left
---

# GitHub "Delete Repo" Bug 
Users could be tricked into visiting a malicious site that auto-submitted a delete form to Github

- [Article](https://www.cve.news/cve-2023-5498/)
- CSRF: Cross-Site Request Forgery is when a user is tricked into performing an unwanted action on a web application where they were already authenticated
- CSRF abuses the automatic sending of cookies by browsers (ex: session tokens) with every request, even if the request comes from another site

---
layout: image-right
transition: slide-left
image: /assets/swizec.png
backgroundSize: 400px 270px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 💻 [Darknet Diaries](https://darknetdiaries.com/)
- 😱 [Spooky Web Dev Horror Stories 1](https://syntax.fm/show/840/spooky-web-dev-horror-stories-part-1)
- 💀 [Spooky Web Dev Horror Stories 2](https://syntax.fm/show/841/spooky-web-dev-horror-stories-part-2)
- 🙀 [Coding Horror Stories 2023 1](https://syntax.fm/show/683/spooky-coding-horror-stories-2023-part-1)
- 👻 [Coding Horror Stories 2023 2](https://syntax.fm/show/684/spooky-coding-horror-stories-2023-part-2)
- ⬛ [Sneakers: Black box scene](https://www.youtube.com/watch?v=EKuwyH1UeYw)

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)


---
transition: slide-left
---

# Authentication vs Authorization
What's the difference?

- Authentication: Verifying who a user is
   - Tools: Passport.js, OAuth
   - Should: use strong passwords, Multi-Factor Authentication (MFA), session expiration
- Authorization: Determining what the user can do (ex: admin vs regular user)
   - Tools: use middleware to role check, [winston](https://www.npmjs.com/package/winston) or morgan for logging
   - Should: consistently examine who can access what? keep a record (audit trail) of system events and actions, Log logins, data changes, access denials, etc. 
