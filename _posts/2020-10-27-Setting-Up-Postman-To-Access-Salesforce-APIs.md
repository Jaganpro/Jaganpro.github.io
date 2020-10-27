---
layout: post
title: Setting up Postman to Access Salesforce APIs
subtitle: 
tags: [Postman, API, OAUTH, Web-Server Flow]
comments: true
nav-short: true
readtime: true
---

## What is Postman?

* A Postman is an HTTP Client Tool.
* It is a software which lets you create/edit HTTP Requests.
* It has capability to create multiple **Collections** - Ie, Group of Requests.
* It has capability to create multiple **Environments** - Ability to save different environments variables with values.
* It has capability to create multiple **Workspaces** - Collection, Environments, Requests can all be grouped into Workspace.

## Installing Postman Salesforce APIs

## Step 1: Download Postman

* [Postman Download Link](https://www.postman.com/downloads/)

<img src="https://user-images.githubusercontent.com/2145211/97129307-e98c4180-1714-11eb-8be2-91997c1eed4d.png" width="600">

## Step 2: Create a New Workspace

<img src="https://user-images.githubusercontent.com/2145211/97130686-77b5f700-1718-11eb-96f5-588596058ccd.png" width="600">

* Create a new Workspace called `Salesforce APIs`
* Choose `Personal` Option. If you are using Paid Product, `Teams` are available to share environments.

<img src="https://user-images.githubusercontent.com/2145211/97130752-a502a500-1718-11eb-8130-502db8d5ff05.png" width="600">

## Step 3: Import Collections

* You can download Salesforce API Collection JSON File here: [Salesforce API Collection](https://github.com/scolladon/Salesforce-Postman/releases/latest/download/salesforce-apis.postman_export.json)

* Drag and drop this JSON file here.

<img src="https://user-images.githubusercontent.com/2145211/97131151-c1531180-1719-11eb-982f-77b33118b6fc.png" width="600">

* You will be able to see all the Salesforce APIs imported in the left Nav Bar.

<img src="https://user-images.githubusercontent.com/2145211/97132641-f82b2680-171d-11eb-914b-d392665959a0.png" width="600">

* You can learn more about Salesforce API collection from this Github Repository: [Postman Salesforce APIs](https://github.com/forcedotcom/postman-salesforce-apis)

## Step 4: Clone and Create your new Environment

* Since we imported the JSON file, you would notice that `Salesforce Template Environment` is created for us.

<img src="https://user-images.githubusercontent.com/2145211/97132688-12fd9b00-171e-11eb-922e-5af671e2f491.png" width="600">

* Clone `Salesforce Template Environment` and create a new `Dev Environment`

<img src="https://user-images.githubusercontent.com/2145211/97133094-35dc7f00-171f-11eb-86b9-a5ad4ec46dbd.png" width="600">


## Step 5: Update Variables from newly created Environment

<img src="https://user-images.githubusercontent.com/2145211/97134974-590a2d00-1725-11eb-9153-8245aeb4671b.png" width="600">

## Step 6: Authentication

* There are many ways to Authenticate to Salesforce Environments. 
* Most popular ways are the follows:
  * SOAP Login Flow (Requires Username + Password + Security Token) [Most Easiest Way]
  * OAUTH Web Server Flow (Requires Connected App Setup + Client ID + Client Secret + Redirect URI) [Most Secure Way, Requires one-time setup]

* My method of Choice is `OAUTH Web-Server Flow`. The reason are of follows:
  * It is a secure authentication mechanism compared to SOAP Login as Username and Password are not stored in Postman.
  * Ability to revoke access from Connected App and track Postman usage from Connected App Usage Page.
  * This is compatible with all Salesforce Environments where SAML SSO is enabled (Salesforce acting as a Service Provider)
    * NOTE: When Salesforce is not the Identity Provider, Passwords are not generated for Users. This forces us to use OAUTH flows for Authentication.
  * OAUTH using Connected App can be scoped based on the permissions we set in our App.

* It consist of 6 distinct Steps.
  * A. Creating Connecte App in Salesforce Environment **(One-Time Setup)**
  * B. Updating Client ID, Client Secret, Redirect URI in Postman Environment Variables **(One-Time Setup)**
  * C. Getting Authorization Code from Salesforce using Web-Server Flow 1 [GET] **(One-Time Setup)**
  * D. Authenticating Postman from Browser **(One-Time Setup)**
  * E. Getting token from Salesforce using Web-Server Flow 2 [POST] **(One-Time Setup)**
  * F. Get a new Access Token using Refresh-Token Flow [POST] **(Everytime when a new Access Token is required)**

### Step 6.A : Creating Connecte App in Salesforce Environment

* Go to Setup --> App Manager --> Create a new Connected App
* Name: `Postman Connect App`
* Redirect URI: `https://www.postman.com/oauth2/callback`
* Scope: Select Appropriate scope as required.

<img src="https://user-images.githubusercontent.com/2145211/97248985-536e1f00-17d9-11eb-82df-f3e4512535e2.png" width="600">

###  Step 6.B : Updating Client ID, Client Secret, Redirect URI in Postman Environment Variables

* Updating Client ID, Client Secret, Redirect URI in Postman Environment Variables

<img src="https://user-images.githubusercontent.com/2145211/97249719-d479e600-17da-11eb-9d61-7d60d063c7bf.png" width="600">

* **NOTE:** Don't worry about other Variables for now. Once we execute our first OAUTH Step, most of the variables will be automatically populated.

###  Step 6.C : Getting Authorization Code from Salesforce using Web-Server Flow 1

* Navigate to Salesforce APIs > Auth Section in Postman
* Choose `Web Server Flow 1` and click SEND.

<img src="https://user-images.githubusercontent.com/2145211/97250791-0ab86500-17dd-11eb-9f0b-41db7a11d8fc.png" width="600">

* You should receive a HTML response from Postman. (I can't figure out a way to make Postman redirect directly in Browser. Hence this step is required for now)
* Click on the Link `Command + Click` to open it in a new Browser Tab.

<img src="https://user-images.githubusercontent.com/2145211/97250890-43f0d500-17dd-11eb-9623-d8c06b6ff901.png" width="600">

### Step 6.D : Authenticating Postman from Browser

* You will be redirected to the Browser and Salesforce will request user to "Allow Access" to "Postman Connected App" 
* Choose "Allow" and then you will be redirected to `Redirect URI` with code embedded as configured in Connected App.

### Step 6.E : Getting token from Salesforce using Web-Server Flow 2

* From the URL, copy the following code `aPrxPT6EtEwV_A4JG28CckI6bzLaR5OOHdKdjK.RgQ7FLQpviXGA4GlN.h_GPo8CWHDXlWX18g%3D%3D`
* Replace `%3D%3D` with `==` as we are converting encoded String before pasting the code in Postman.
* Final Code should look like `aPrxPT6EtEwV_A4JG28CckI6bzLaR5OOHdKdjK.RgQ7FLQpviXGA4GlN.h_GPo8CWHDXlWX18g==`

<img src="https://user-images.githubusercontent.com/2145211/97250963-6a167500-17dd-11eb-8dce-2b34f067625c.png" width="600">

* Paste the Code under Body of Postman `code`

<img src="https://user-images.githubusercontent.com/2145211/97253434-c6c85e80-17e2-11eb-93e0-3fc6fddd6a8a.png" width="600">

* Before we send Post Request, add the following line of code to `Test` section. 
  * `pm.environment.set("_refreshToken", jsonData.refresh_token);`
  * Variables starting with `_` are private variables and they will be automatically populated in Environment Variables when an request is executed.
  * If `_refreshToken` variable does not exist, please create one under Environment Variables.

<img src="https://user-images.githubusercontent.com/2145211/97253516-01ca9200-17e3-11eb-8f4f-9b5dd89a4df4.png" width="600">

* After the Request is executed, we should be able to get the `access_token`, `refresh_token`, `instance_url`, `id` - all being autopopulated in Environment Variables for future use.

<img src="https://user-images.githubusercontent.com/2145211/97254299-705c1f80-17e4-11eb-95f7-88f738b6984f.png" width="600">

### Step 6.F : Get a new Refresh Token when a new Session Id is required

* That's it!! If you are successful up until this step, you are good to execute any API request as `Access Token` is automatically stored under Environment Variables.
* If the `Access Token` is expired, just execute `Refresh Token Flow` under Auth in Left Nav Bar.

<img src="https://user-images.githubusercontent.com/2145211/97255592-5a9c2980-17e7-11eb-97ee-19c70c8d951f.png" width="600">

## Sample API Request using POSTMAN

<img src="https://user-images.githubusercontent.com/2145211/97255772-c5e5fb80-17e7-11eb-8260-5557be33f0c5.png" width="600">

## Credits:
* Developer Evangelist from Salesforce @@PhilippeOzil
* Open Source Github Repo Link: https://github.com/forcedotcom/postman-salesforce-apis
