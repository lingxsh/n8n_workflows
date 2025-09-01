
---

## ✅ Your scenario in plain words

1. A user signs up in **your application** with name + email.
2. That signup should be captured by **n8n**.
3. n8n should then send a **welcome email** to the user’s email.

👉 The missing piece you’re struggling with is: **How does n8n know about the signup that happened inside your app?**

---

## 🔑 There are only 2 possible ways for this to work:

---

### **Option 1 – Your Application Sends Data to n8n via Webhook**

* Your app backend (the code that processes signup) is modified so that **after it stores the user in database**, it makes a `POST` request to the **n8n Webhook URL**.
* Example flow:

  1. User signs up → your app calls **your own signup API** (internal).
  2. Your app backend saves user → then sends the same signup data to **n8n webhook URL**:

     ```
     POST https://n8n.yourserver.com/webhook/signup
     Body:
     {
       "name": "Lingesh",
       "email": "lingesh@example.com"
     }
     ```
  3. n8n receives that data → you connect **Send Email node** → user gets welcome email.

✅ Here:

* **Webhook URL is the bridge** between your application and n8n.
* You don’t call your signup API from n8n. Instead, your app itself notifies n8n.

---

### **Option 2 – n8n Calls Your Signup API (if webhook support is not possible)**

* Instead of your app sending data, you make n8n itself handle the signup.

  1. Some trigger in n8n (manual, schedule, or another webhook) starts the flow.
  2. n8n makes a `POST` request to your **Signup API endpoint** with user’s name + email.

     ```
     POST https://api.yourapp.com/signup
     Body:
     {
       "name": "Lingesh",
       "email": "lingesh@example.com",
       "password": "*****"
     }
     ```
  3. The API responds with success and user details.
  4. n8n passes that user’s email to **Send Email node** → sends welcome email.

✅ Here:

* **n8n directly talks to your signup API**.
* You don’t use n8n webhook at all.

---

## 🔍 So which one applies to you?

From your words:

> “A user sign-up in our application with login credentials as email and name … that email and name should … be used in n8n to send welcome email”

That means:

* The signup **already happens inside your application**, not inside n8n.
* Therefore, n8n needs a way to **receive that signup info from your app**.
* The only way is → your app backend must **send the signup details to n8n Webhook URL**.

👉 That’s why I said Webhook URL is enough. It’s not magic — your developers must **connect your app’s signup flow to the n8n webhook** by calling it whenever a user signs up.

---

## 🚦 What you need to do

1. In n8n, create a **Webhook node** (Production URL).
   Example: `https://n8n.yourserver.com/webhook/signup`
2. Ask your application developer (backend team) to **call this webhook URL** after a successful signup, passing email + name.
3. In n8n, connect the Webhook node → Email node → use `{{$json["email"]}}` as recipient.
4. Done ✅ Every new signup will trigger n8n automatically.

---

💡 Simple analogy:

* **Signup API** = the door your users use to enter your app.
* **Webhook URL** = the bell your app rings to tell n8n “Hey, someone just came in!”.

Without ringing the bell (webhook), n8n will never know who signed up.

---
