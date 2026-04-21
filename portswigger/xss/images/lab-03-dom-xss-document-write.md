# 🔐 PortSwigger Lab - DOM XSS in document.write Sink Using Source location.search

## 🧠 Day 3 Learning

Today I learned how DOM-based XSS occurs when JavaScript reads attacker-controlled input from the URL and writes it directly into the page.

Key concepts learned:

* Client-side JavaScript execution
* DOM sink vulnerability
* Source-to-sink flow analysis

---

## 📸 Lab Description

![Lab Description](images/lab-03-dom-xss.png)

---

## 🔍 Step 1: Understanding the Application Logic

The application contains a search functionality.

The search term from the URL is processed by JavaScript.

The page uses:

```javascript id="dom1102"
document.write()
```

to insert search input into HTML.

👉 User input is taken directly from:

```javascript id="dom1103"
location.search
```

---

## 📸 Initial Application View

![Initial Page](images/lab-03-dom-xss-initial-page.png)

---

## ⚠️ Vulnerability

The application writes URL-controlled data into HTML without sanitization.

Because of this:

Attacker input becomes executable DOM content.

---

## 🧠 Source and Sink

### Source

```javascript id="dom1104"
location.search
```

### Sink

```javascript id="dom1105"
document.write()
```

---

## ⚔️ Step 2: Initial Testing

Entered random input:

```text id="dom1106"
test123
```

Inspected page source.

Observed input reflected inside image source attribute.

Example:

```html id="dom1107"
<img src="/resources/images/tracker.gif?searchTerms=test123">
```

---

## 📸 Inspecting Sink

![Inspect Sink](images/lab-03-dom-xss-inspect-sink.png)

---

## 🧠 Exploitation Strategy

Because input is placed inside an attribute:

Need to break attribute context.

---

## ⚔️ Step 3: Inject Payload

Used payload:

```html id="dom1108"
"><svg onload=alert(1)>
```

---

## 📸 Payload Injection

![Payload Injection](images/lab-03-dom-xss-payload.png)

---

## 🔥 Why This Works

Payload closes current attribute:

```html id="dom1109"
"
```

Then injects:

```html id="dom1110"
<svg onload=alert(1)>
```

The browser executes JavaScript when SVG loads.

---

## ⚔️ Step 4: Trigger Execution

Loaded payload through search parameter.

The alert executed immediately.

---

## 📸 Alert Triggered

![Alert Triggered](images/lab-03-dom-xss-alert.png)

---

## 🎯 Result

Lab solved successfully.

Payload used:

```html id="dom1111"
"><svg onload=alert(1)>
```

---

## 🏁 Final Result

Successfully:

* Exploited DOM XSS
* Identified source and sink
* Broke attribute context
* Triggered JavaScript execution
* Solved the lab

---

## 🧠 Key Learnings

* DOM XSS happens entirely in browser
* Source and sink must be identified clearly
* Payload depends on exact injection context

---

## 🔐 Vulnerability Type

* DOM-Based Cross-Site Scripting
* Unsafe DOM Manipulation

---

## 🛡️ Prevention

* Avoid unsafe sinks like document.write()
* Use safe DOM APIs
* Sanitize URL input before rendering

---

## 🔥 Real Insight

> DOM XSS can exist completely in frontend JavaScript without server-side reflection.
