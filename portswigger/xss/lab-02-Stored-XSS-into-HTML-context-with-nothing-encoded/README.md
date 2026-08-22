# 🔐 PortSwigger Lab - Stored XSS into HTML Context with Nothing Encoded

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Vulnerability:** Stored Cross-Site Scripting (XSS)

---

# 🧪 Lab Overview

This lab contains a **Stored Cross-Site Scripting (XSS)** vulnerability in the comment functionality.

The application allows users to submit comments on blog posts. These comments are stored by the application and displayed when the blog post is viewed.

The objective of the lab was to submit a comment containing JavaScript that calls the `alert()` function when the blog post is viewed.

The lab name is:

**Stored XSS into HTML context with nothing encoded**

The main objective was to understand how user-controlled input submitted through the comment section was handled, stored, and later displayed.

---

# 🧠 How I Approached the Problem

Instead of directly assuming that the comment functionality was vulnerable, I followed a step-by-step testing approach.

My approach was:

```text
Access the Lab
        ↓
Explore the Home Page
        ↓
Identify a Blog Post
        ↓
Open the View Post Page
        ↓
Find the Comment Section
        ↓
Test HTML Input
        ↓
Check Whether the Comment Is Stored
        ↓
Observe How the Input Is Displayed
        ↓
Test an XSS Payload
        ↓
Revisit the Blog Post
        ↓
Refresh the Page
        ↓
Confirm Persistent JavaScript Execution
```

The important thing I wanted to understand was:

> **Does the application store my comment and later display it without proper output encoding?**

---

# 📸 Screenshot 1: Understanding the Lab

The first screenshot shows the lab information provided by PortSwigger Web Security Academy.

The lab is titled:

**Stored XSS into HTML context with nothing encoded**

The lab description explains that the application contains a **Stored Cross-Site Scripting (XSS)** vulnerability in the comment functionality.

The objective was to submit a comment that calls the `alert()` function when the blog post is viewed.

This gave me the initial understanding that the main area to investigate was the **comment functionality**.

![Screenshot 1](images/screenshot-1-Understanding%20the%20Lab.png)

---

# 📸 Screenshot 2: Accessing the Lab

After clicking **Access the Lab**, the application opened the home page.

At this stage, I started exploring the available functionality to identify possible user-controlled input points.

Since the lab description mentioned a vulnerability in the comment functionality, I needed to locate a blog post where comments could be submitted.

![Screenshot 2](images/screenshot-2-Accessing%20the%20Lab.png)

---

# 📸 Screenshot 3: Identifying the View Post Option

I scrolled through the web page and explored the available options.

During this process, I found the **View Post** option.

Since comments are usually associated with individual blog posts, I clicked on **View Post** to explore the page and check whether it contained a comment functionality.

My next question was:

> **Does this blog post allow users to submit comments that may later be displayed to other users?**

![Screenshot 3](images/screenshot-3-Identifying%20the%20View%20Post%20Option.png)

---

# 📸 Screenshot 4: Finding the Comment Section and Testing HTML Input

After entering the blog post through the **View Post** option, I scrolled down to explore the page.

I found a **comment section** where users could submit their comments.

At this point, I considered that any comment submitted through this functionality might be stored by the application and later displayed when the blog post was viewed.

To understand how the application handled HTML input, I entered the following text into the comment field:

```html
<h1>Cyber Security</h1>
```

The purpose of this test was to determine whether the application would treat my input as:

- Plain text
- Actual HTML

This step helped me understand whether special characters and HTML tags were being properly encoded before being displayed.

![Screenshot 4](images/screenshot-4-Finding%20the%20Comment%20Section%20and%20Testing%20HTML%20Input.png)

---

# 📸 Screenshot 5: Submitting the Comment

After entering the HTML input, I clicked the **Post Comment** button.

The application displayed the message:

```text
Thank you for your comment!
```

This indicated that the application had successfully accepted and processed my comment.

At this stage, I wanted to verify whether the submitted input had been stored and how it would be displayed when the comment section was viewed again.

![Screenshot 5](images/screenshot-5-Submitting%20the%20Comment.png)

---

# 📸 Screenshot 6: Confirming the Stored HTML Input

When I returned to the comment section, I found that the text I submitted was displayed in the comments.

The HTML input:

```html
<h1>Cyber Security</h1>
```

was interpreted by the browser instead of being displayed as harmless plain text.

This indicated that the HTML tags were not properly encoded before being displayed.

This was an important observation because if user-controlled HTML could be interpreted by the browser, I considered the possibility that JavaScript might also be executed.

At this point, the application behavior suggested the following flow:

```text
User Input
     ↓
Comment Submitted
     ↓
Comment Stored
     ↓
Stored Comment Displayed
     ↓
Browser Interprets HTML
```

This led me to investigate whether the same behavior could result in a Stored XSS vulnerability.

![Screenshot 6](images/screenshot-6-Confirming%20the%20Stored%20HTML%20Input.png)

---

# 🔍 Different Possibilities I Considered

During the testing process, I considered the following questions:

- Is my comment accepted by the application?
- Is the submitted comment stored?
- Does the comment remain available when the page is viewed again?
- Is my input displayed as plain text or interpreted as HTML?
- Are special characters such as `<` and `>` being encoded?
- Can HTML tags be injected into the comment section?
- If HTML is interpreted, can JavaScript also be executed?
- Does the payload execute again when the blog post is revisited or refreshed?

My testing process was:

```text
Identify Comment Functionality
        ↓
Submit HTML Input
        ↓
Check Whether the Comment Is Stored
        ↓
View the Stored Comment
        ↓
Observe HTML Interpretation
        ↓
Test JavaScript Payload
        ↓
Return to the Blog
        ↓
Verify JavaScript Execution
        ↓
Refresh the Page
        ↓
Confirm Persistent Execution
```

This approach helped me understand the application's behavior instead of immediately submitting an XSS payload.

---

# 📸 Screenshot 7: Injecting the XSS Payload

After observing that my HTML input was interpreted by the browser, I tested whether JavaScript execution was also possible.

I entered an XSS payload into the comment field:

```html
<script>alert("Hacked")</script>
```

The purpose of this test was to determine whether the application would store the JavaScript payload and later allow the browser to execute it when the stored comment was displayed.

After entering the payload, I clicked the **Post Comment** button.

![Screenshot 7](images/screenshot-7-Injecting%20the%20XSS%20Payload.png)

---

# 📸 Screenshot 8: Confirming the Comment Submission

After clicking the **Post Comment** button, the application displayed the message:

```text
Thank you for your comment!
```

This confirmed that the application had accepted the submitted comment.

However, because this was a **Stored XSS** test, the important step was not only submitting the payload.

I needed to check what would happen when the stored comment was later loaded and displayed.

![Screenshot 8](images/screenshot-8-Confirming%20the%20Comment%20Submission.png)

---

# 📸 Screenshot 9: Confirming the XSS Execution

After submitting the payload, I clicked the **Back to Blog** button.

When the stored comment was displayed, a popup appeared showing:

```text
Hacked
```

This confirmed that the JavaScript contained in the stored comment was executed by the browser.

The application had:

```text
Accepted the Input
        ↓
Stored the Input
        ↓
Displayed the Stored Input
        ↓
Browser Interpreted the Code
        ↓
JavaScript Executed
```

This confirmed the presence of a **Stored Cross-Site Scripting (XSS)** vulnerability.

![Screenshot 9](images/screenshot-9-Confirming%20the%20XSS%20Execution.png)

---

# 📸 Screenshot 10: Testing Whether the XSS Was Persistent

To further verify the behavior, I refreshed the page.

The purpose of this test was to determine whether the payload had only executed once or whether it remained stored and continued to execute whenever the vulnerable content was loaded.

This is an important characteristic of **Stored XSS** because the malicious input is saved by the application.

![Screenshot 10](images/screenshot-10-Testing%20Whether%20the%20XSS%20Was%20Persistent.png)

---

# 📸 Screenshot 11: Confirming Persistent Stored XSS

After refreshing the page, the popup displaying:

```text
Hacked
```

appeared again.

This confirmed that the payload remained stored and was executed again whenever the vulnerable page was loaded.

Therefore, the vulnerability was confirmed as:

> **Stored Cross-Site Scripting (Stored XSS)**

The important observation was that the injected input did not disappear after the initial submission.

Instead, it remained stored by the application and continued to execute when the stored content was displayed.

![Screenshot 11](images/screenshot-11-Confirming%20Persistent%20Stored%20XSS.png)

---

# ⚡ How the Vulnerability Was Confirmed

The vulnerability was confirmed by submitting the following XSS payload through the comment functionality:

```html
<script>alert("Hacked")</script>
```

The application accepted the comment and displayed:

```text
Thank you for your comment!
```

When the stored comment was later displayed, JavaScript was executed and a popup appeared showing:

```text
Hacked
```

I then refreshed the page to verify whether the behavior was persistent.

After refreshing, the popup appeared again.

This confirmed that:

```text
The input was stored
        ↓
The stored input was displayed
        ↓
The browser interpreted the injected code
        ↓
JavaScript executed
        ↓
The payload executed again after refresh
```

This behavior confirmed a **Stored Cross-Site Scripting vulnerability**.

---

# 🎯 Root Cause

The vulnerability occurred because the application accepted **user-controlled input through the comment functionality**, stored it, and later displayed it in an HTML context without proper output encoding.

The vulnerable flow can be understood as:

```text
User-Controlled Comment
        ↓
Comment Submission
        ↓
Application Stores the Input
        ↓
Stored Comment Retrieved
        ↓
Input Displayed Without Proper Encoding
        ↓
Browser Interprets the Input
        ↓
JavaScript Executes
```

The main security issue was that untrusted user input was treated as HTML rather than being safely displayed as text.

The fact that the input was stored in the application did not make it trusted.

> **Stored user-controlled data must still be treated as untrusted data whenever it is displayed.**

---

# 🛡️ Remediation

To prevent this type of Stored XSS vulnerability, the following security measures should be implemented.

## 1. Context-Aware Output Encoding

User-controlled comments should be properly encoded before being inserted into an HTML page.

Special characters such as:

```text
< > " '
```

should be displayed as text rather than being interpreted as HTML or executable code.

This is the primary defense against XSS.

---

## 2. Treat Stored Data as Untrusted

Data should not automatically be considered safe simply because it has been stored in a database.

The secure flow should be:

```text
User Input
     ↓
Stored in Database
     ↓
Still Considered Untrusted
     ↓
Encode Before Displaying
```

Every time user-controlled data is displayed, it should be handled according to the output context.

---

## 3. Use Secure Frameworks and Templating Systems

Modern frameworks and templating systems can automatically escape user-controlled content.

Developers should use these security features correctly and avoid bypassing automatic output encoding.

This helps prevent browsers from interpreting user input as executable HTML or JavaScript.

---

## 4. Avoid Unsafe HTML Rendering

Applications should avoid directly inserting untrusted user input into HTML.

User-controlled comments should be displayed as text unless there is a specific and securely implemented requirement to allow limited HTML.

---

## 5. Input Validation

Input validation can help restrict unexpected input and enforce expected formats.

However:

> **Input validation alone is not a complete defense against XSS.**

Proper context-aware output encoding is still required.

---

## 6. Content Security Policy

A properly configured **Content Security Policy (CSP)** can provide an additional layer of protection.

CSP can help reduce the impact of certain XSS vulnerabilities by restricting where scripts can be loaded or executed from.

However, CSP should be considered a **defense-in-depth measure** and not a replacement for proper output encoding.

> **The main remediation is to properly encode untrusted user-controlled output according to the context where it is displayed.**

---

# 📚 Key Learning

This lab helped me understand the difference between **Reflected XSS** and **Stored XSS**.

## Reflected XSS

In Reflected XSS, the user-controlled input is typically reflected immediately in the application's response.

```text
User Input
     ↓
Application Response
     ↓
Input Reflected
     ↓
Possible JavaScript Execution
```

## Stored XSS

In Stored XSS, the user-controlled input is first stored by the application and later displayed.

```text
User Input
     ↓
Application Stores Input
     ↓
Input Remains Stored
     ↓
Page Is Viewed Later
     ↓
Stored Input Is Displayed
     ↓
Possible JavaScript Execution
```

Through this lab, I learned about:

- Stored Cross-Site Scripting (XSS)
- Persistent storage of user-controlled input
- Comment functionality as a potential input point
- Testing HTML input before testing JavaScript execution
- How browsers interpret unencoded HTML
- The difference between Reflected XSS and Stored XSS
- Why stored data should still be considered untrusted
- Context-aware output encoding
- Persistent JavaScript execution
- The importance of testing the application after refreshing or revisiting a page

---

# 🎓 My Biggest Takeaway

> **User-controlled input does not become safe simply because it has been stored in a database. The data must still be properly handled and encoded whenever it is displayed.**

Another important lesson from this lab was:

> **Understanding the complete flow of user input—from submission, to storage, to display—is essential when testing for Stored XSS vulnerabilities.**

---

# 🏁 Conclusion

This lab demonstrated a **Stored Cross-Site Scripting (XSS)** vulnerability in a blog comment functionality.

By progressively testing:

```text
Explore the Application
        ↓
Identify the Comment Section
        ↓
Test HTML Input
        ↓
Confirm the Input Is Stored
        ↓
Observe HTML Interpretation
        ↓
Submit an XSS Payload
        ↓
Return to the Blog
        ↓
Observe JavaScript Execution
        ↓
Refresh the Page
        ↓
Confirm Persistent Execution
```

I was able to understand how user-controlled input was stored and later displayed without proper output encoding.

This ultimately allowed the browser to interpret and execute injected JavaScript.

This lab reinforced an important web security principle:

> **Never trust user-controlled input, even after it has been stored. Always apply context-appropriate output encoding before displaying it in a web page.**

---

# 🧠 Learning Summary

Through this lab, I learned about:

- Stored Cross-Site Scripting (XSS)
- Persistent XSS vulnerabilities
- User-controlled input in comment functionality
- HTML context injection
- The difference between Stored XSS and Reflected XSS
- How stored data can affect future page loads
- The importance of checking whether input persists
- Context-aware output encoding
- Secure handling of untrusted data
- Basic Stored XSS remediation techniques

---

# 🔐 Vulnerability Type

- **Stored Cross-Site Scripting (Stored XSS)**
- **Improper Output Encoding**
- **Persistent XSS**
- **HTML Context Injection**

---

# ⚠️ Disclaimer

This lab was completed in the **PortSwigger Web Security Academy**, an intentionally vulnerable and authorized learning environment.

The techniques demonstrated in this repository are documented strictly for:

- Educational purposes
- Hands-on cybersecurity learning
- Understanding web application vulnerabilities
- Authorized security testing

Do not use these techniques against systems without proper authorization.

---

# 👨‍💻 Author

**PALAGIRI GURU CHANDRAYUDU**

Cybersecurity Student | Web Application Security Enthusiast