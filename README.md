# SellAuth Shopify style Sale Notifications

Create **Shopify style sale notifications** for your SellAuth store and receive them instantly on your phone.

This project uses:

* SellAuth
* Pushover
* Pipedream

Example notification:

```
€29.99, 1 product from Online Store
• YOURSHOPNAME
```

---

# What This Does

1. Customer completes a purchase in SellAuth
2. SellAuth sends a webhook notification
3. Pipedream receives the webhook
4. Pipedream fetches invoice details from SellAuth
5. Pipedream sends a push notification via Pushover

Result: **Instant Shopify-style sale notification on your phone.**

---

# 1) What You Need First

Before starting, make sure you have:

• A SellAuth account
• A Pushover account
• A Pipedream account
• Your SellAuth **API key**
• Your **Shop ID**

---

# 2) Setup Pushover

Install the **Pushover app** on your phone and create an account.

Go to:

```
https://pushover.net/apps/build
```

Create a new application.

Example settings:

```
Name: SellAuth Notifications
```

---

# 3) Add a Custom ICON

Inside your Pushover application settings upload a Icon

Recommended:

• Shopify logo styled with SellAuth colors looks the best 


This gives notifications a **clean Shopify style appearance**.

After creating the application copy:

```
API Token / Key
```

Also copy your:

```
User Key
```

You need it later 


# 4) Add a Custom Sound

Upload a custom notification sound.

Recommended name:

```
shopify
```

You can use the Shopify notification sound included in this repo.

Then notifications will play the Shopify sound.

---

# 5) Create a Pipedream Workflow

Go to:

```
https://pipedream.com
```

Create a new workflow in the dash go to Sources

Select:

```
HTTP / Webhook
→ New Requests
```

This generates a webhook URL like:

```
https://xxxx.m.pipedream.net
```

---

# 6) Connect SellAuth Webhook

Open your SellAuth dashboard.

Navigate to:

```
Settings
→ Notifications
```

Enable HTTP notifications for:

```
Order Completed
```

Paste your **Pipedream webhook URL**.

Now every completed order will trigger your workflow.

---

# 7) Fetch Invoice Data

Add a step in Pipedream.

Name the step exactly:

```
custom_request1
```

Method:

```
GET
```

URL:

```
https://api.sellauth.com/v1/shops/YOUR_SHOP_ID/invoices/{{steps.trigger.event.body.data.invoice_id}}
```

Authorization:

```
Bearer YOUR_SELLAUTH_API_KEY
```

This step retrieves:

• product name
• quantity
• price
• payment method
• location

---

# 8) Send Notification to Pushover

Add another step called:

```
pushover_request
```

Settings:

Method

```
POST
```

URL

```
https://api.pushover.net/1/messages.json
```

Content-Type

```
application/x-www-form-urlencoded
```

Body fields:

```
token = YOUR_PUSHOVER_API_TOKEN
user = YOUR_USER_KEY
title = New Sale
Message = €{{steps.custom_request1.$return_value.items[0].total_price}}, {{steps.custom_request1.$return_value.items[0].quantity}} {{steps.custom_request1.$return_value.items[0].quantity == 1 ? "product" : "products"}} from Online Store
 • YOURSTORENAME
sound = shopify
```

---

# 9) Deploy the Workflow

Click:

```
Deploy
```

Your notifications are now live.

Every new order will send a push notification to your phone.

---

# Example Notification

```
NEW SALE

€29.99 — 1 product from Online Store
• YOURSTORENAME
```

---

# Customization

You can easily change what data appears in the notification.

Examples:

Product name

```
{{steps.custom_request1.$return_value.items[0].product.name}}
```

Price

```
{{steps.custom_request1.$return_value.items[0].total_price}}
```

Country

```
{{steps.custom_request1.$return_value.country_code}}
```

Payment method

```
{{steps.custom_request1.$return_value.gateway}}
```

---

# Important

Step names must match exactly.

Example:

```
custom_request1
```

If you rename the step you must update the variables in the notification message.

---

# Optional Testing

You can simulate notifications with random values:

```
€{{(Math.random()*40+10).toFixed(2)}}, {{Math.floor(Math.random()*8)+1}} products from Online Store
 • YOURSTORENAME
```

---

# Final Notes

You can customize the notification however you want.

If you want the **original Shopify notification style**, keep the message format used in this guide.

# Need help join my design studio we will help :} 
https://discord.com/invite/2nCRP2TQzN
