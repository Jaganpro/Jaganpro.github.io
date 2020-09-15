---
layout: post
title: Salesforce Contact Center Implementation using Service Cloud Voice
subtitle: Amazon Connect + Service Cloud Voice Architect Notes and Considerations
tags: [Amazon Connect, Service Cloud Voice, Service Cloud, Salesforce]
comments: true
---

## Contact Center Implementation

{: .box-note}
**Note:** Opinions expressed here are my own and not affiliated with the company i work for.

* I have been heavily involved in helping our IT team and our business partners in choosing the right technology to re-implement Contact Center. Its been an exciting journey so far as i am learning so much along the way. On a high level, you are better off picking a cloud based technology stack and give more preference to vertical integration. This seems like a easy choice, but in the world of telephony, until recently, its been hard to implement 100% cloud based solution.

* That changed when Salesforce introduced [Amazon Connect](https://aws.amazon.com/connect/) integration behind as [Service Cloud Voice](https://www.salesforce.com/products/service-cloud/solutions/call-center-management/) in Summer '20.

* This is the first time Telephony is getting a huge upgrade! It seems like as if Telephony is moving digital. And that's really exciting!

* Based on the Business requirements, i have gathered the following topics needed for Contact Center Implementation.

## Project Scope

* Project Scope can be broadly classified into the following topics:
  * Telephony Infrastructure
    * TollFree Support
    * DID Support
    * Outbound Call
    * UIFN Support
    * Call Quality Guarantee etc.
  * Why Amazon Connect?
  * Why Salesforce Service Cloud Voice?
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

* The scope of this can vary widely based on business requirements. Some Questions to consider:
  * Is your Call Center based out of United States?
  * Single Call Center vs Multi/Distributed Call Center
  * Call Centers being Geographically distributed (Within or Outside US)
  * CallCenter Business Hours (24/7 or 24/5.5 or 8/7 etc.)
  * Does your Call Center need Toll-Free Support?
  * Does your Call Center need DID and Outbound Call Support?

* But Before we jump into talking about Telephony, we got to clarify few basic terminologies:

#### What is a DID Number?
* Direct Inward Dialing (DID) is a telephone service that allows a phone number to ring through directly to a specific phone at a business

#### What is a Toll-Free Number?
* A toll-free telephone number or freephone number is a telephone number that is billed for all arriving calls instead of incurring charges to the originating telephone subscriber

#### What is ITFS?
* International Toll-Free Service (ITFS) enables directly-dialled international calls to be made from fixed and mobile phones free of international charges to the callers. Full charges are paid by the called subscriber. Different ITFS numbers will be assigned for each country.

#### What is Universal International Freephone Number (UIFN)?
* Universal International Freephone Number (UIFN) – one single unique number that can be dialed from different countries to reach the same customer.

{: .box-warning}
**Warning:** UIFN is not supported in all countries

* List of Supported Countries as of Sep 2020: 
  * [Global UIFN Service Provide List](https://www.itu.int/en/ITU-T/inr/unum/Pages/uifn-service-provider.aspx) 
  * Note: Even if a country is supported with UIFN, not all carriers within the country is supported.
  
#### How to choose between ITFS/UIFN/DTF?
* I found this handy guide to help us decide when to use ITFS vs UIFN vs DTF

<p align="center">
  <img width="300" src="https://user-images.githubusercontent.com/2145211/93224676-5fc57d80-f73f-11ea-9943-0c0256d0bcc2.png">
</p>

* Credit: [Toll Free Numbers Guide](https://cdn.avoxi.com/wp-content/uploads/2018/11/eBook-Toll-Free-Numbers-Guide.pdf) 

#### How to provide support for TollFree number if existing or new Provider does not have native TollFree support?

{: .box-warning}
**Warning:** As of Sep 2020, Amazon Connect does not support Native TollFree Numbers from India and China

* If TollFree Number is not Supported OOTB for Amazon, It is possible to purchase external TollFree Number to bridge gap.
* As of this writing (Sep 2020) 231 Countries are currently supported for Outbound Calls with Amazon Connect.
  * Check out the latest offerings from Amazon Connect here: [Amazon Connect Pricing](https://aws.amazon.com/connect/pricing/)
* Given this conditions, we can use services such as Avoxi to create Virtual TollFree Numbers in India and China (For Example) and Call forward to any Location.
* Once native support is available from AWS Connect, services from Avoxi can be deprecated for one-stop shop simplified billing.
* Example Service Provider: [Avoxi](https://www.avoxi.com/) 

{: .box-note}
**Note:** I am not affliated with Avoxi. If you find a better resource, please feel free to comment below.

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
    

