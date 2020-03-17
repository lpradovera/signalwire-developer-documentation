---
layout: page
title: Introduction to LaML
exclude: true
---

# What is LaML?

LaML is the language used by SignalWire to determine how the phone numbers in your account react during calls or text messages. When a call or message comes into one of your SignalWire phone numbers, SignalWire makes an HTTP request to the URL endpoint you configured for that number. Your response to that request instructs SignalWire on what to do next.

Responses to the HTTP request are in an XML format we call LaML. LaML allows you to specify instructions using simple LaML commands called verbs. SignalWire starts at the top of your LaML document and executes your LaML commands in order, from top to bottom.

# How LaML works

When n message or call is received by one of your SignalWire phone numbers, SignalWire lookup the URL endpoint you configured for that phone number and make an HTTP request to it. The URL endpoint then responds to the request with a LaML document that determines what to do next.

SignalWire will read the instructions in the LaML document, from top to bottom, and execute the commands in order.

You can generate and respond with raw XML yourself, or utilize one of the [SignalWire helper libraries](https://docs.signalwire.com/topics/laml-api/#laml-rest-api-client-libraries-and-sdks) to help you generate it easily. To get started, we will be making use of SignalWire LaML Bins, our serverless hosted service that allows you to easily serve static documents without setting up any infrastructure.

To find out how to set up your account and your first LaML bin, refer to [SignalWire 101](/intros/signalwire_101)

Outbound calls and messages started via the [LaML REST API](https://docs.signalwire.com/topics/laml-api) are controlled the same way. When you start an outbound call or message, you also pass a link to a LaML document. SignalWire will make a request to this document to determine how it should proceed with the call or message.

While a single LaML document is executed at a time, many documents can be linked together and generated dynamically to create complex applications to fit any need.

Documentation on LaML is available [on the SignalWire website](https://docs.signalwire.com/topics/laml-xml/#laml-xml-specification), and you will find more information there.

Our goal for this article will to create simple practical interactions to showcase the LaML bulding blocks and features.

## Call forwarding and Mustache templating

A common use case is to forward an incoming call to another phone number. For example, you might not want to give out your personal phone number, or you would like to be able to handle calls in different ways than just connecting them.

The following LaML will result in all calls being forwarded to the specified number, which in the example is a Washington State weather service.

{% gist 62a8ffe5ee07f50aa901c0cad6c778ad %}

You will notice a specific tag being used in this document, `{{From}}` as the value of the `callerId` property.

That is a Mustache templating variable that is used to make your LaML Bin dynamic. Any variable that is part of the HTTP payload for the request, whether it is one of the[standard parameters](https://docs.signalwire.com/laml-xml/#voice-laml-overview-request-parameters) or a query string variable you set (`https://yourspace.signalwire.com/laml-bins/your-laml-bin-id?YOURVARIABLE=123`) will be available for replacement.

In this case, we are using the `From` value so that the caller ID for the forwarded call echoes the original calling number. In this way, your calls will have the correct caller identification.

SignalWire allows you to set your caller ID to three types of numbers: 
- The caller ID the call is coming from, such as in this example
- A number you own on your SignalWire account
- A phone number you have verified with your SignalWire account

This provides flexibility while protecting our users from fraud.