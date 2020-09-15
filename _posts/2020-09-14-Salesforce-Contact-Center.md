---
layout: post
title: Salesforce Contact Center
subtitle: Amazon Connect + Service Cloud Voice Architect Notes and Considerations
tags: [Amazon Connect, Service Cloud Voice, Service Cloud]
comments: true
---

## Contact Center Implementation

* I have been heavily involved in helping our IT team and our business partners in choosing the right technology to re-implement Contact Center. Its been an exciting journey so far as i am learning so much along the way. On a high level, you are better off picking a cloud based technology stack and give more preference to vertical integration. This should seem like a easy choice, but in the world of telephony, until recently, its been hard to implement 100% cloud based solution.

* That changed when [Amazon Connect](https://aws.amazon.com/connect/) and [Service Cloud Voice](https://www.salesforce.com/products/service-cloud/solutions/call-center-management/) was introduced in Summer '20.

* This is the first time Telephony is getting a huge upgrade! And that's really exciting!

* Based on the Business requirements, i have gathered the following topics needed for Contact Center Implementation.

## Project Scope

* Project Scope can be broadly classified into the following topics:
  * Telephony Infrastructure
    * TollFree Support
    * DID Support
    * Outbound Call
    * UIFN Support
    * Call Quality Guarantee etc.
  * IVR(Interactive Voice Response)/ACD(Automatic Call Distribution)/Call Flow
  * Salesforce Features
    * SoftPhone Application
    * OmniChannel Setup
    * Queue (or) External (or) Skill Based Routing
    * Real-Time Transcription
    * Next Best Actions (Automatic Suggestion based on Business Process)
  * QM (Quality Management)
    * Interactive Coaching (Whisper)
    * Accessing Call Recordings from Cases
    * Screen & Voice Recording
  * Analytics
  * WFM (Workforce Management)
  
## Telephony Infrastructure

* <More to add here!>

## IVR/ACD/Contact Flows

* Here are the list of IVR Patterns used across fortune 500 Companies.
* The list below is aggregation of several call center IVR implementation patterns including `The New York Times`, `Intuit (TurboTax)`, `NewsCorp`, `American Express` and `Salesforce`. Thanks to Amazon ReInvent Sessions! Increadibly helpful!

| Feature | Description |
|--|--|
|Language Support|Do our Call Center Support multi languages? If yes, options for caller to choose appropriate Language |
|AI for Speech Recognition | Primarily used for Call deflections. Understanding user voice and route based on context configured |
|Skill Based Routing Based on Call Flows| Based on Caller request, routing to appropriate Agent/Queue |
|Data Dip in CallFlows from Salesforce| Uses AWS Lamda to dynamically lookup caller to provide intelligent routing |
|Keypad or # for Selecting Menu| Caller Presses Numbers to select various options configured. Could be used for Entitlements/ Valet Service |
|Business Hours| Automatic/Intelligent response when caller calls outside of business hours|
|Confirm your Identity (Verification)| IVR Attempts to confirm you Identity so that Agents can focus on building relationships. (DOB or PII to lookup back into Salesforce) |
|Welcome Message + Compliance Message| Standard Compliance Message for Call Recording Purpose and CFA Welcome Message |
|CallBack Experience| When all agents are busy, caller can schedule time so that agents can call back |
|Direct Inward Dial | Agents can provide DID with Extension to Callers to skip regular toll-free Number|
|Survey After Call | IVR asks permission from caller to opt in for call experience/feedback|
| Self Service actions without Agents Involvement | True Call Deflection. Identifying common scenarios agents takes call and providing automated options (Such as Reset Password) to be actioned with call without talking to Agents |
|Call Identification | Identifying and Logging Location of the Caller based on Phone Number|
|Wait Time | Estimated Wait Time in the Queue Calculation  |
    
* <More to add here!>
