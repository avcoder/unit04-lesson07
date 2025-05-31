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
- [ ] Principles and Attack Vectors
- [ ] Defending your app

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

# Heartland Data Breach 2009
Through SQL Injection, the company's computers were compromised, and 100 million debit/credit cards were stolen

- [Article 1](https://www.twingate.com/blog/tips/Heartland%20Payment%20Systems-data-breach)
- [Article 2](https://www.secureworks.com/blog/general-pci-compliance-data-security-case-study-heartland)


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
