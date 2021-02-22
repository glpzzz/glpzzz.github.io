---
title: "Special links for our websites"
date: 2021-02-21
description: "How to use regular <a> links for custom tasks as calls, sms or
email?"
image: https://apimania.netlify.app/api/txt2img?text=Special%20links%20for%20our%20websites&font=Titillium%20Web&format=4:3
---
Sometimes we need our links to do something else besides going to another http
location. Perhaps we want to allow visitor to use other ways to interact with
us using email, sms, a call, or others. Let's se how to.

## Email

Very easy! `<a href="mailto:me@domain.com">Send me an email</a>` will generate an
anchor that, when clicked, will tell the browser to open the application in
charge of handling email on your system.

More "complex" examples:

- Specify the subject: `<a href="mailto:me@domain.com?subject=Hello">Send email</a>`
- Specify the subject and body: `<a href="mailto:me@domain.com?subject=Hello&body=This%20is%20a%20message">Send email</a>`

More details on [RFC 6068 - The mailto URI Scheme](https://tools.ietf.org/html/rfc6068#section-6).

## SMS

A little different:

- Just open the SMS app: `<a href="sms:">Send SMS</a>`
- Open the SMS app for a certain number: `<a href="sms:+1-555-111-1111">Send SMS</a>`
- Open the SMS app with custom message: `<a href="sms:+1-555-111-1111?body=I%20want%this%20product">Request Product</a>`

More details on [RFC 5724 - URI Scheme for Global System for Mobile Communications Short Message Service (SMS)](https://tools.ietf.org/html/rfc5724#section-2.5).

## Phone Calls

Same idea here:

- Open the dial app on the device: `<a href="tel:+1-555-111-1111">Call me</a>`

More details on [RFC 3966 - The tel URI for Telephone Numbers](https://tools.ietf.org/html/rfc3966#section-6) and in Section 8 is adviced to always enclose the
phone number inside the `<a>` tag, so the correct way for previous example should be something like:

`Call me at <a href="tel:+1-555-111-1111">1-555-111-1111<a>.`

## Locations

If you want the link to open a map in a certain location:

- `<a href="geo:22.146147,-80.4426516">Our Office</a>`

More details on [RFC 5870 - A Uniform Resource Identifier for Geographic Locations](https://tools.ietf.org/html/rfc5870#section-6.2).

## Conclusion

In all cases is required that your device have a correct user agent to handle
this URI schemes. In PC the tel, sms, and geo will normally not work. But in
smartphone the potencial of this feature is amazing. Use it!

