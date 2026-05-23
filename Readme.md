# Complete ngrok Setup Guide for Webhook Development

# Why Developers Use ngrok

When developing applications locally, your server usually runs like this:

```txt
http://localhost:3000
```

This works only on your computer.

However, third-party services such as:

- Stripe
- GitHub
- Razorpay
- Slack
- Telegram
- PayPal

need a **public HTTPS URL** to send webhook requests.

They cannot access:

```txt
localhost
```

because localhost exists only inside your own machine.

---

# The Main Problem

Suppose your local application is:

```txt
http://localhost:3000/webhook
```

And Stripe wants to send payment notifications.

Stripe servers are on the internet.
Your localhost server is private.

So this will fail:

```txt
Stripe → localhost
```

---

# What ngrok Does

ngrok creates a secure public tunnel between the internet and your local machine.

Flow:

```txt
Internet
   ↓
ngrok public HTTPS URL
   ↓
Secure encrypted tunnel
   ↓
Your localhost application
```

Example:

```txt
https://abcd1234.ngrok-free.app
          ↓
http://localhost:3000
```

Now any third-party service can communicate with your local application.

---

# Real-World Use Cases

Developers commonly use ngrok for:

| Use Case               | Example          |
| ---------------------- | ---------------- |
| Payment webhooks       | Stripe, Razorpay |
| GitHub webhooks        | Push events      |
| OAuth callbacks        | Google login     |
| Telegram bots          | Bot webhooks     |
| Mobile app API testing | Android/iOS apps |
| Client demos           | Share local app  |
| Testing APIs           | External access  |

---

# Step-by-Step ngrok Setup

---

# Step 1 — Create a Local Server

Example using Node.js and Express.

## Create project

```bash
mkdir webhook-demo
cd webhook-demo
```

---

## Initialize project

```bash
npm init -y
```

---

## Install Express

```bash
npm install express
```

---

## Create server.js

```javascript
const express = require("express");

const app = express();

app.use(express.json());

app.post("/webhook", (req, res) => {
  console.log("Webhook received:");
  console.log(req.body);

  res.sendStatus(200);
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

---

## Run server

```bash
node server.js
```

Output:

```txt
Server running on port 3000
```

Your application is now running at:

```txt
http://localhost:3000
```

Webhook endpoint:

```txt
http://localhost:3000/webhook
```

---

# Step 2 — Download ngrok

Go to:

[https://ngrok.com/downloads](https://ngrok.com/downloads)

Download for your operating system.

Supported platforms:

- Windows
- macOS
- Linux

---

# Step 3 — Install ngrok

## Windows

Extract ZIP file.

Move ngrok.exe somewhere easy.

Example:

```txt
C:\ngrok\ngrok.exe
```

---

## macOS

Using Homebrew:

```bash
brew install ngrok/ngrok/ngrok
```

---

## Linux

Example:

```bash
sudo snap install ngrok
```

Or download manually.

---

# Step 4 — Create ngrok Account

Go to:

[https://dashboard.ngrok.com/signup](https://dashboard.ngrok.com/signup)

Create a free account.

Why account is needed:

- Authentication
- Tunnel management
- Usage tracking
- Dashboard access
- Better reliability

---

# Step 5 — Get Authentication Token

After login:

Open:

[https://dashboard.ngrok.com/get-started/your-authtoken](https://dashboard.ngrok.com/get-started/your-authtoken)

You will see something like:

```bash
ngrok config add-authtoken 2abCDEfgHIJK
```

Run this command in terminal.

Example:

```bash
ngrok config add-authtoken 2abCDEfgHIJK
```

This connects your local machine to your ngrok account.

---

# Step 6 — Verify ngrok Installation

Run:

```bash
ngrok version
```

Example output:

```txt
ngrok version 3.x.x
```

---

# Step 7 — Start ngrok Tunnel

Your local app is running on port:

```txt
3000
```

Open another terminal.

Run:

```bash
ngrok http 3000
```

---

# Step 8 — Understand ngrok Output

You will see something like:

```txt
Session Status                online
Account                       your-email@gmail.com
Version                       3.x.x
Region                        India (in)
Forwarding                    https://abcd1234.ngrok-free.app -> http://localhost:3000
```

Important line:

```txt
https://abcd1234.ngrok-free.app
```

This is your public HTTPS URL.

---

# Step 9 — Connect Webhook URL

Suppose your local webhook endpoint is:

```txt
http://localhost:3000/webhook
```

Then your public webhook URL becomes:

```txt
https://abcd1234.ngrok-free.app/webhook
```

Use this inside:

- Stripe webhook settings
- GitHub webhook settings
- Razorpay dashboard
- Slack events setup
- Telegram bot webhook setup

---

# Step 10 — Test Webhook

Example flow:

```txt
Stripe Payment
      ↓
Stripe sends webhook
      ↓
ngrok HTTPS URL
      ↓
localhost:3000/webhook
      ↓
Your Express app receives data
```

Console output:

```txt
Webhook received:
{
  "event": "payment.success"
}
```

---

# ngrok Request Inspection Dashboard

ngrok provides a debugging dashboard.

Open:

```txt
http://127.0.0.1:4040
```

You can inspect:

- Incoming requests
- Headers
- Request body
- Responses
- Status codes
- Errors

This is extremely useful for webhook debugging.

---

# Understanding the Internal Flow

Without ngrok:

```txt
Internet cannot access localhost
```

With ngrok:

```txt
Third-party service
        ↓
ngrok public server
        ↓
Encrypted tunnel
        ↓
Your local machine
        ↓
localhost application
```

ngrok acts like a secure bridge.

---

# Common ngrok Commands

## Start HTTP tunnel

```bash
ngrok http 3000
```

---

## Start HTTPS tunnel

```bash
ngrok http https://localhost:3000
```

---

## TCP tunnel

```bash
ngrok tcp 22
```

Useful for SSH access.

---

## View help

```bash
ngrok help
```

---

## View tunnel status

```bash
ngrok tunnels list
```

---

# Example with Stripe Webhook

Suppose Stripe webhook URL is:

```txt
https://abcd1234.ngrok-free.app/webhook
```

When payment succeeds:

```txt
Customer pays
     ↓
Stripe event generated
     ↓
Stripe sends POST request
     ↓
ngrok receives request
     ↓
Forwards to localhost
     ↓
Your app processes payment event
```

---

# Example with GitHub Webhook

GitHub push event flow:

```txt
Developer pushes code
        ↓
GitHub webhook triggered
        ↓
ngrok public URL
        ↓
localhost server
        ↓
CI/CD or automation starts
```

---

# Advantages of ngrok

| Feature             | Benefit             |
| ------------------- | ------------------- |
| HTTPS support       | No SSL setup needed |
| Public URL          | Internet accessible |
| Tunnel security     | Encrypted traffic   |
| Easy setup          | Beginner friendly   |
| Debugging dashboard | Inspect requests    |
| Works instantly     | Fast development    |

---
# Paid ngrok Features

Paid plans provide:

- Fixed domains
- Reserved subdomains
- Team collaboration
- Better uptime
- Multiple tunnels
- Advanced security

---

# Security Notes

When using ngrok:

- Do not expose sensitive admin endpoints
- Avoid sharing public URLs publicly
- Validate webhook signatures
- Use authentication where possible

Example:

Stripe provides webhook signature verification.

Always validate incoming requests.

---

# Alternative Tools

Other tunneling tools:

| Tool                | Notes           |
| ------------------- | --------------- |
| Cloudflare Tunnel   | Free and stable |
| LocalTunnel         | Lightweight     |
| Tunnelmole          | Open source     |
| Serveo alternatives | Community tools |

---

# Why ngrok Became Popular

Before tools like ngrok:

Developers needed:

- Public servers
- VPS setup
- SSL certificates
- DNS configuration
- Firewall setup

ngrok simplified everything into:

```bash
ngrok http 3000
```

This made webhook development much easier.

---

# Typical Modern Development Flow

## During development

```txt
Local App
    ↓
ngrok tunnel
    ↓
Third-party webhook services
```

---

## In production

```txt
Internet
    ↓
Real HTTPS domain
    ↓
Cloud server
    ↓
Production backend
```

---

# Best Practice Recommendation

For beginners:

- Use ngrok
- Learn webhook flow
- Use request inspection dashboard

For production:

- Use real HTTPS domains
- Deploy on cloud infrastructure
- Configure SSL properly

---

# Quick Setup Summary

## Start local server

```bash
node server.js
```

---

## Start ngrok

```bash
ngrok http 3000
```

---

## Copy generated HTTPS URL

Example:

```txt
https://abcd1234.ngrok-free.app
```

---

## Add webhook endpoint

```txt
https://abcd1234.ngrok-free.app/webhook
```

---

## Test webhook

Send test event from Stripe/GitHub/etc.

---

# Final Understanding

ngrok is mainly used because:

```txt
localhost is private
```

and webhooks require:

```txt
public HTTPS access
```

ngrok solves this by creating:

```txt
A secure public tunnel to your local machine
```

This allows developers to test real webhook integrations directly from their local environment without deploying their application to a public server.
