---
title: Kapcharge API Reference

# language_tabs: # must be one of https://git.io/vQNgJ
#   - shell
#   - c#

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kapcharge API

toc_footers:
  - <span>&copy;</span>Kapcharge by Kapital Solutions.
  - All Rights Reserved.
  - v2.0.0 | <a href='https://www.kapital.solutions/terms'>Terms and Conditions</a> | <a href='https://www.kapital.solutions/privacy'>Privacy Policy</a>
---
<a name="top"></a>
# Introduction
<p><a target="_blank" href="https://www.kapcharge.com" target="_blank" style="color: #84d404;">Kapcharge</a> provides a full suite of payment processing products by <a target="_blank" href="https://www.Kapital.Solutions" target="_blank" style="color: #3cb5d0;">Kapital Solutions</a> that support online lenders, eCommerce, and subscription businesses.</p>
<p>Kapcharge provides businesses of all sizes with software and APIs that enable them to accept payments, send payouts, and manage their financial transactions.</p>
<p>Kapcharge furnishes payment processing services through a single API that allows businesses to connect seamlessly with the banking and payment ecosystem. We provide our customers with the most secure and reliable payment processing service in <b>Canada</b> and the <b>United States</b>, backed by risk management and data analytics.</p>

## Financial Systems in Canada and the United States

<img src="/images/FinancialSystemOverview.png"
     alt="Financial System Overview"
     style="float: left; margin-right: 10px;" />

## References & Concepts in Virtual Terminal in both Canada and the United States


Kapcharge (Canada)| Kapcharge (USA)|Description
--------- | -----------|---------
Institution Number||Customer Institution/Bank Number (maximum 3 characters).
Transit Number|Routing Number|In Canada, your routing number is a combination of the 5-digit branch number and the 3-digit financial institution number that can be found on the bottom of your personal cheques. In the U.S. a routing number is a 9-digit number. (maximum 50 characters).
Account Number|Account Number|Customer Account Number (maximum 12 characters).
Card Number|
CVV|
Expiry Month|
Expiry Year|
Transaction Type| | "Charge-Receivable/ Credit-Payable" in both US and Canada.
Transaction Method| |"EFT" only.
<br/>
<p>In addition to providing a high-level overview of the API, this document also outlines how to call it successfully.</p>

# <span class="kapcharge-highlight">Kapcharge</span> Payment Gateway


## Transaction Flow

The transaction flow takes place among three key stakeholders as shown below

<img src="/images/MovingFundsOverview.png"
     alt="Transaction Flow Overview"
     style="float: left; margin-right: 10px;" />

## Transaction Types

<aside class="notice">
Run in Postman button automatically stay updated with changes in the Kapcharge original API collection, so you always get the most recent version of Kapcharge API collection without having to manually update the collection's link.
</aside>

## Payment Methods

### United States

<img src="/images/USPaymentOverview.png"
     alt="US Payment Methods Overview"
     style="float: left; margin-right: 10px;" />

### Canada

Method|	Description|Duration|Send Transaction|Request Transaction
--------- | -----------|--------|------------|--------
EFT|Electronic funds transfer between banks|3 times a day|Yes|Yes
Interac|Online e-transfers, user receives an e-mail or SMS|Immediately *|Yes|Yes
Visa Direct|Visa rails to send and pull funds directly to visa debit card	|Immediately|Yes|Yes*
Credit Card|Credit Card payments/checkout to collect funds	|Immediately	|No	|Yes

<img src="/images/PaymentMethodsOverview.png"
     alt="Payment Methods Overview"
     style="float: left; margin-right: 10px;" />

# API Specification

## API Introduction

This technical integration specification document explains the KAPCHARGE Payment Gateway new API. It enables merchant systems to post real-time transaction requests to KAPCHARGE Payment Gateway.

This part of the documentation discusses the technical requirements for real-time integration only. For specifications relating to Batch File Uploading or using Virtual Terminals to create and send the transactions online, please refer to the other services of the Kapcharge.

## API Specifics
* All API requests must pass the header Content-type: application/json; unless explicitly mentioned.

<aside class="warning">
The secret key has a <b>one-year validity limit</b> effective from the day you received the key and applies to all instances in the API wherein we refer to {{ClientSecret}}. <br/> You will be notified prior <b>one month</b> before the expiration of secret key with the new {{ClientSecret}}.
</aside>

## Postman Collection

Follow these steps to quickly get started with the Kapcharge API:

Once you have access to your staging environment:

1) Login in your staging environment,Grab your api {{ClientID}} and {{ClientSecret}}

2) Download and install the Postman app

3) Install the Kapcharge Postman Collection. Click the "Run in Postman" button below to install

4) In Postman find the endpoint /OAuth GET AccessToken within Authentication folder, paste your api {{ClientID}} and {{ClientSecret}} and it's all set.

<a target="_blank" href="https://god.gw.postman.com/run-collection/18538588-9d0a6b1c-fc9c-4d71-b34c-b9ddc9d6f682?action=collection%2Ffork&amp;collection-url=entityId%3D18538588-9d0a6b1c-fc9c-4d71-b34c-b9ddc9d6f682%26entityType%3Dcollection%26workspaceId%3Dbceb4dfb-9362-4a88-9174-f6595e89dc8c"><img src="https://run.pstmn.io/button.svg" alt="Run in Postman"></a>

<aside class="notice">
Run in Postman button automatically stay updated with changes in the Kapcharge original API collection, so you always get the most recent version of Kapcharge API collection without having to manually update the collection's link.
</aside>

## <span class="kapcharge-highlight">Kapcharge</span> Swagger API 
 <a href="https://api-staging.kapcharge.com/swagger/index.html" target="_blank" class="kapcharge-btn">Swagger API</a>

## Authentication

<img src="/images/AuthenticationAPIOverview.png"
     alt="Financial System Overview"
     style="float: left; margin-right: 10px;" />

### OAuth GET AccessToken
Bearer Token Authorization is used for all API requests. Multiple API calls can be made securely without requiring authorisation each time with a Bearer Token.

### Payload

> Authentication Payload

```shell
{
  "clientId":{{ClientID}}
  "clientSecret":{{ClientSecret}}
}
```


### HTTP Request
`Method: POST`<br/>
`Endpoint: {{Application URL}}/api/v1/OAuth/token`


> Make sure to replace `{{Application URL}}` with API Application URL as per the environment. 

```shell
curl --location -g --request POST '{{Application URL}}/api/v1/OAuth/token' 
```

> The above command returns JSON structured like this:

```json
{
    "accessToken": "{JWT Token}",
    "tokenType": "Bearer",
    "expiresIn": 3600,
    "expiresOn": 1666298431
    
}
```
<aside class="notice">
You must replace <code>{{Application URL}}</code> with API Application URL as per the environment.
</aside>

Parameter | Type| Mandatory 
--------- | ------- |------- 
clientId |string| yes 
clientSecret |string| yes 

<aside class="notice">
Please contact your Account Manager to obtain clientId and clientSecret or <a href="https://kapcharge.com/ach-payments-usa/#callback" target="_blank" style="color: #84d404;">Contact Us</a>
</aside>


### Responses

Parameter | Description
--------- | ------- 
accessToken| The authorization token that needs to be used throughout the API request.
tokenType| This will always be "Bearer" token type.
expiresIn| This is token expiration time (seconds).
expiresOn| This is token validation end-time (epoch time).
## EFT Transactions

### Create Transactions

<img src="/images/CreateTransactionAPI.png"
     alt="Create Transaction API Overview"
     style="float: left; margin-right: 10px;" />

### EFT Payload

> EFT Payload

```shell
[	
    {	
        "MerchantId": "TEST_123-KC",	
        "Currency": "CAD",	
        "ClientReference": "JohnDoe",	
        "AmountInCents": 100,	
        "DueDate": "2022-11-10",	
        "TransactionFlow": "DEBIT",	
        "TransactionMethod": "EFT",	
        "Customer": {	
            "FirstName": "John",	
            "LastName": "Doe",	
            "AddressLine1": "1234 Est",	
            "AddressLine2": "Testfield",	
            "City": "Montreal",	
            "Country": "CA",	
            "Province": "QC",	
            "PostalCode": "12345",	
            "Email": "test@test.com",	
            "PhoneNumber": "5141231234"	
        },	
        "CustomerBankAccount": {	
            "InstitutionName": "ABC Bank",	
            "InstitutionNumber": "123",	
            "AccountNumber": "1234567",	
            "RoutingNumber": "12345678910",	
            "TransitNumber": "1234567",	
            "AccountType": "PC",	
            "AccountOwnershipType": "INDIVIDUAL"	
        }	
    }
]
```

### HTTP Request
`Method: POST`<br/>
`Endpoint: {{ Application Url }}/api/v1/transaction`

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
For any parameter related to Country ,please use the <b>alpha-2 standard</b>.<br/>Follow the link below:<br/> 
</aside>

<a href="https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes" target="_blank" style="color: #84d404;">ISO Country Codes</a>

### Transaction Request

Parameter | Type| Mandatory| Default | Description
--------- | ------- |------- |------- | -----------
MerchantId |string|yes|  | Merchant id (maximum 15 characters).
Currency |string|yes| | Either "CAD" or "USD" only (maximum 3 characters).
ClientReference|string|yes|| Client Reference (maximum 100 characters).
AmountInCents|integer|yes|| Transaction amount in cents.
DueDate|string|yes||Due date for the transaction (Format: YYYY-MM-DD).
TransactionFlow|string|yes|| Either "DEBIT" or "CREDIT" only.
TransactionMethod|string|yes|EFT| "EFT" only.
Customer|<a href="#customer" style="color: #84d404;">object</a>|yes|  | Pass the Customer object with all the required details as seen in Customer below.
ShippingInformation|<a href="#shipping-information" style="color: #84d404;">object</a>|no| | Pass the Shipping Information object with all the required details as seen in Shipping Information below.
CustomerBankAccount|<a href="#customer-bank-account" style="color: #84d404;">object</a>|yes| | Pass the Customer Bank Account object with all the required details as seen in Customer Bank Account below.

#### Customer

Parameter | Type| Mandatory|  Description
--------- | ------- |------- | -----------
FirstName |string|yes|First Name of the customer (maximum 100 characters).
LastName |string|yes|Last Name of the customer (maximum 100 characters).
AddressLine1 |string|yes|Billing Address Line 1 (maximum 255 characters).
AddressLine2 |string|no|Billing Address Line 2 (maximum 255 characters).
City|string|yes|Billing address city (maximum 100 characters).
Country |string|yes|Billing address country (maximum 4 characters).Refer notice below.
Province |string|no|Billing address state/province (maximum 100 characters).
PostalCode |string|yes|Billing address postal code (maximum 20 characters).
Email |string|yes|Customer email (maximum 100 characters).
PhoneNumber |string|yes|Phone number (maximum 50 characters, no special characters accepted).
SocialSecurityNumber |string|no|Social Security Number of the customer (maximum 50 characters).
IdentificationType |string|no|Identification Type of the customer, check Identification Type <a href="#identification-type" style="color: #84d404;">here</a>.
IdentificationNumber |string|no|Identification Number of the customer (maximum0 characters).
IdentificationCountry |string|no|Identification country.Refer notice below.
IdentificationProvince |string|no|Identification state/province.
IdentificationExpirationDay |string|no|Identification Expiration Day of the month.
IdentificationExpirationMonth |string|no|Identification Expiration Month of the year.
IdentificationExpirationYear |string|no|Identification Expiration Year.



```shell
curl --location -g --request POST '{{ Application Url }}/api/v1/transaction' 
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.
> For any parameter related to Country ,please use the <b>alpha-2 standard</b>.<br/>Follow the link below:<br/> 
> [ISO Country Codes](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes).

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
For any parameter related to Country ,please use the <b>alpha-2 standard</b>.<br/>Follow the link below:<br/> 
</aside>
<a href="https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes" target="_blank" style="color: #84d404;">ISO Country Codes</a>

> The above command returns JSON structured like this:<br/>
> Status Code : 200 OK<br/>

```json
[
    {
        "kapchargeTransactionId": 12345679,
        "clientReference": "JohnDoe",
        "transactionStatus": "Accepted",
        "transactionFlow": "DEBIT",
        "amountInCents": 100,
         "errors": [
            {
                "message": ""
            }
         ]
    }
]
```
> Status Code : 400 Bad Request<br/>

```json
[
    {
        "message": ""
    }
]
```

> Status Code : 403 Forbidden<br/>

```json
    {
        "message": ""
    }
```

#### Shipping Information 

Parameter | Type| Mandatory| Description
--------- | ------- |------- | ----------
ShippingCarrier |string|no| Shipping Carrier Details (maximum 100 characters).
ShippingTrackingNumber |string|no| Shipping Tracking Number (maximum 50 characters).
ShippingMethod|string|no| Shipping Method (maximum 50 characters).
ShippingFirstName |string|no|First Name of Shipping user (maximum 100 characters).
ShippingLastName |string|no|Last Name of Shipping user (maximum 100 characters).
ShippingAddressLine1 |string|no|Shipping Address Line 1 (maximum 255 characters).
ShippingAddressLine2 |string|no|Shipping Address Line 2 (maximum 255 characters)
ShippingCity |string|no|Shipping address city (maximum 100 characters).
ShippingProvince |string|no|Shipping address state/province (maximum 100 characters).
ShippingCountry |string|no| Shipping address country (maximum 4 characters).Refer notice below.
ShippingPostalCode |string|no|Shipping address postal code (maximum 20 characters).
ShippingPhoneNumber |string|no|Phone number (maximum 50 characters, no special characters accepted).
ShippingEmailAddress |string|no|Shipping email (maximum 100 characters).

#### Customer Bank Account 

Parameter | Type| Mandatory|  Description
--------- | ------- |------- | -----------
InstitutionName |string|yes| Customer Institution/Bank Name (maximum 100 characters).
InstitutionNumber |string|yes|Customer Institution/Bank Number (maximum 3 characters).
AccountNumber |string|yes|Customer Account Number (maximum 12 characters).
RoutingNumber |string|yes| In Canada, your routing number is a combination of the 5-digit branch number and the 3-digit financial institution number that can be found on the bottom of your personal cheques. In the U.S. a routing number is a 9-digit number. (maximum 50 characters).
TransitNumber |string|yes|Customer Transit Number (maximum 50 characters).
AccountType |string|yes|Check Account Type <a href="#account-type" style="color: #84d404;">here</a>.
AccountOwnershipType |string|yes| Either BUSINESS or INDIVIDUAL only.

#### Account Type

Type | Description
--------- | ------- 
PC |Personal Checking
PS |Personal Savings
PL |Personal Loan
BC |Business Checking
BS |Business Savings
BL |Business Loan

#### Identification Type
Type | Description
--------- | ------- 
DL | Driver’s License
SS | Government ID
MI | Military ID
GN | Generic ID

### Responses

Parameter | Description
--------- | ------- 
kapchargeTransactionId| The unique transaction ID generated by Kapcharge.
clientReference|  Transaction reference provided by the client.
transactionStatus|Refer KAPCHARGE Bank Status Response Codes <a href="#kapcharge-transaction-status-codes" style="color: #84d404;">here</a>.
transactionFlow| Either "Debit" or "Credit".
amountInCents| Transaction amount in cents.
listOfErrors| Error message (only when there is some error processing request).



### Get a specific transaction

<img src="/images/GetTransactionAPI.png"
     alt="Get Transaction API Overview"
     style="float: left; margin-right: 10px;" />

### 
> Get a specific transaction

```shell
curl --location -g --request GET '{{ Application Url }}/api/v1/transaction/12345679' 
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.

### HTTP Request
`Method: GET`<br/>
`Endpoint: {{ Application Url }}/api/v1/transaction/<TransactionID>`

> The above command returns JSON structured like this:

```json
{
    "kapchargeTransactionId": 12345679,
    "clientReference": "SmithDoe",
    "transactionStatus": "Accepted",
    "transactionFlow": "DEBIT",
    "amountInCents": 200,
    "transactionEvents": [
        {
            "eventDate": "2022-10-25T17:46:38.791Z",
            "eventType": "SUBMITTED",
            "description": "CREATED_BY_EFT_FILE_JOB"
        },
        {
            "eventDate": "2022-10-25T15:36:30.265Z",
            "eventType": "CREATED"
        }
    ]
}
```


<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
</aside>



This endpoint retrieves a specific transaction request based on transaction id.

### Responses

Parameter | Description
--------- | ------- 
kapchargeTransactionId| The unique transaction ID generated by Kapcharge.
clientReference|Transaction reference provided by the client.
transactionStatus|Refer KAPCHARGE Bank Status Response Codes <a href="#kapcharge-transaction-status-codes" style="color: #84d404;">here</a>.
transactionFlow|Either "Debit" or "Credit".
amountInCents| Transaction amount in cents.


### Delete a specific transaction

### HTTP Request
`Method: DELETE`<br/>
`Endpoint: {{Application Url}}/api/v1/transaction/<TransactionID>`


### Delete a specific transaction

<img src="/images/DeleteTransactionAPI.png"
     alt="Delete Transaction API Overview"
     style="float: left; margin-right: 10px;" />

### 

### HTTP Request
`Method: DELETE`<br/>
`Endpoint: {{Application Url}}/api/v1/transaction/<TransactionID>`

> Delete a specific transaction

```shell
curl --location -g --request DELETE '{{Application Url}}/api/v1/transaction/12345679' 
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
</aside>

> The above command returns JSON structured like this:

```json
{
    "kapchargeTransactionId": 12345679,
    "clientReference": "12345679",
    "transactionStatus": "Cancelled",
    "transactionFlow": "DEBIT",
    "amountInCents": 200
}
```

This endpoint deletes a specific transaction request based on transaction id.

### Responses

Parameter | Description
--------- | ------- 
kapchargeTransactionId| The unique transaction ID generated by Kapcharge.
clientReference|Transaction reference provided by the client.
transactionStatus|Refer KAPCHARGE Bank Status Response Codes <a href="#kapcharge-transaction-status-codes" style="color: #84d404;">here</a>.
transactionFlow|Either "Debit" or "Credit".
amountInCents| Transaction amount in cents.

<aside class="notice">
For EFT, transactions can only be deleted and cancelled if it's not transmitted to the Bank.
</aside>

#### KAPCHARGE Transaction Status Codes

Description|Status Code|Description
--------- | ----------- |--------------
Accepted|0| Indicates the transaction was sent to the Bank.
InProgress |2| Indicates the transaction is being processed.
Rejected|5| Indicates the transaction was returned by the Bank.
CommunicationFailure|90| Indicates the transaction was accepted by Kapcharge but due to network issue could not reach the Bank.Hence the transaction needs to be tried once again.
Cancelled |-1| Indicated the transaction was deleted by Kapcharge.
Declined |1| Indicated the transaction was declined by Kapcharge as it was not valid.

## Webhooks Configuration
### Overview
Overview of Webhook Configuration API:
 <img src="/images/WebHookAPIOverview.png"
     alt="WebHook Configuration API step-by-Step Overview"
     style="float: left; margin-right: 10px;" />

The webhooks aim to give your organization a real-time update on returned transactions.

### Update a Webhooks configuration for a specific merchant

### Payload

> Update Merchant Webhooks Configuration Payload

```shell
{
  "merchantId": "TEST_123-KC",
  "webhookEnabled": false,
  "webhookUrl": "TEST URL",
  "webhookAuthenticationHeader": "TEST AUTH HEADER"
}
```

### HTTP Request
`Method: PUT`<br/>
`Endpoint: {{Application Url}}/api/v1/configuration`

`GET http://example.com/kittens/<ID>`

To enable webhooks, you must send the below information through the API. 

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
</aside>

Parameter | Description
--------- | ------- 
merchantId| The unique merchant ID generated by Kapcharge.
webhookEnabled| Boolean (true or false),true => Transaction Rejection notification => <b>ON</b>, false => Transaction Rejection notification => <b>OFF</b>.If ON => Notification will be received on the webhook url.
webhookUrl| Merchant webhook url.
webhookAuthenticationHeader| Merchant webhook authentication header.

```shell
curl --location -g --request PUT '{{Application Url}}/api/v1/configuration'
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.

> The above command returns JSON structured like this:

```json
{
    "merchantId": "TEST_123-KC",
    "webhookEnabled": false,
    "webhookUrl": "TEST URL",
    "webhookSecret": "{WebHookSecret}",
    "webhookAuthenticationHeader": "TEST AUTH HEADER",
    "oAuthClientId": "{ClientSecret}"
}
```

This endpoint updates a specific webhooks configuration request based on merchant id.

### Responses

Parameter | Description
--------- | ------- 
merchantId| The unique merchant ID generated by Kapcharge.
webhookEnabled| Boolean (true or false),true => Transaction Rejection notification => <b>ON</b>, false => Transaction Rejection notification => <b>OFF</b>.If ON => Notification will be received on the webhook url.
webhookUrl| Merchant webhook url.
webhookSecret| Please refer to Contract or contact Account Manager to obtain.
webhookAuthenticationHeader| Merchant webhook authentication header.
oAuthClientId| Please refer to Contract or contact Account Manager to obtain.
    
### Get Webhooks Configuration for a specific merchant
Identification
When a URL is defined, Kapcharge will send a POST request each time there is a status change to "rejected".

### HTTP Request
`Method: GET`<br/>
`Endpoint: {{Application Url}}api/v1/configuration/<MerchantID>`

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
</aside>

```shell
curl --location -g --request GET '{{Application Url}}/api/v1/configuration/TEST_123-KC' 
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.

> The above command returns JSON structured like this:

```json
{
    "merchantId": "TEST_123-KC",
    "webhookEnabled": false,
    "webhookUrl": "TEST URL",
    "webhookSecret": "{WebHookSecret}",
    "webhookAuthenticationHeader": "TEST AUTH HEADER",
    "oAuthClientId": "{ClientSecret}"
}
```

This endpoint retrieves a specific webhooks configuration request based on merchant id.

### Responses

Parameter | Description
--------- | ------- 
merchantId| The unique merchant ID generated by Kapcharge.
webhookEnabled| Boolean (true or false),true => Transaction Rejection notification => <b>ON</b>, false => Transaction Rejection notification => <b>OFF</b>.If ON => Notification will be received on the webhook url.
webhookUrl| Merchant webhook url.
webhookSecret|Please refer to Contract or contact Account Manager to obtain.
webhookAuthenticationHeader| Merchant webhook authentication header.
oAuthClientId|Please refer to Contract or contact Account Manager to obtain.

### Webhook Return Notification

<img src="/images/WebHookReturnAPI.png"
     alt="Payment Methods Overview"
     style="float: left; margin-right: 10px;" />

Configure using <a href="#http-request-5" style="color: #84d404;">Webhook Config API</a>

##
> Webhook Return Notification

```json
{
  "transactionId" : 12345678,
  "returnCode" : "{RETURN CODE}",
  "returnDateTime" : "{DATETIME}""
}
```

Kapcharge sends a return notification to merchants once a transaction is returned. The message body that merchants receive is as follows:

#### Transaction Return Codes (For Canada Only)

Return Code   |	Description
------------|------------------
900|	Edit Reject
901|	NSF (Debit Only)
902|	Account not found
903|	Payment Stopped/Recalled
905|	Account Closed
907|	No Debit Allowed
908|	Funds Not Cleared (Debit Only)
909|	Currency/Account Mismatch
910|	Payor/Payee Deceased
911|	Account Frozen
912|	Invalid/Incorrect Account No.
914|	Incorrect Payor/Payee Name
915|	No Agreement Existed
916|	Not According to Agreement - Personal
917|	Agreement Revoked – Personal
918|	No Confirmation/Pre-Notification – Personal
919|	Not According to Agreement – Business
920|	Agreement Revoked – Business
921|	No Confirmation/Pre-Notification –	Business
922|	Customer Initiated Return
990|    Institution in Default
<br/>
For more details kindly refer to: 
<a href="https://www.payments.ca/sites/default/files/standard007eng.pdf" target="_blank" style="color: #84d404;">Payments Canada</a> (LIST OF RETURN REASON CODES ,Page 9-10)

### Webhook Return Notification - Test Simulator

This API is protected, which means the user needs to obtain an access token and include the token in the header when calling the API mentioned above. (like our other APIs). 

```shell
curl --location -g --request POST '{{Application Url}}/api/v1/Transaction/return/12345678' 
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.

### HTTP Request
`Method: POST`<br/>
`Endpoint:{{Application Url}}/api/v1/Transaction/return/<TRANSACTION_ID>`

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
</aside>

###Payload:

The only input required is TRANSACTION_ID. 

### Response: 

* There is no response body
* API returns 200, 401, or 403

> Webhook Return Notification

```json
{
  "transactionId" : 12345678,
  "returnCode" : "{RETURN CODE}",
  "returnDateTime" : "{DATETIME}""
}
```

### How it will work?

* API will look into Merchant's Webhook configuration <br/>(<i>Refer: Update Webhooks Configuration <a href="#update-a-webhooks-configuration-for-a-specific-merchant" style="color: #84d404;">here</a> or Get Webhooks Configuration <a href="#get-webhooks-configuration-for-a-specific-merchant" style="color: #84d404;">here</a></i> ) 
* Post a return message to the merchant's webhook URL. 
* The message body that merchant would receive is as mentioned in Webhook Return Notification <a href="#webhook-return-notification" style="color: #84d404;">here</a>

<aside class="notice">
Users should have enabled webhook and provide a webhook URL using the configuration API.
</aside>

<a class="top-link hide" href="#top" style="text-decoration:none">^</a>

Return Code   |	Description
------------|------------------
900|	Edit Reject
901|	NSF (Debit Only)
902|	Account not found
903|	Payment Stopped/Recalled
905|	Account Closed
907|	No Debit Allowed
908|	Funds Not Cleared (Debit Only)
909|	Currency/Account Mismatch
910|	Payor/Payee Deceased
911|	Account Frozen
912|	Invalid/Incorrect Account No.
914|	Incorrect Payor/Payee Name
915|	No Agreement Existed
916|	Not According to Agreement - Personal
917|	Agreement Revoked – Personal
918|	No Confirmation/Pre-Notification – Personal
919|	Not According to Agreement – Business
920|	Agreement Revoked – Business
921|	No Confirmation/Pre-Notification –	Business
922|	Customer Initiated Return
990|    Institution in Default
<br/>
For more details kindly refer to: 
<a href="https://www.payments.ca/sites/default/files/standard007eng.pdf" target="_blank" style="color: #84d404;">Payments Canada</a> (LIST OF RETURN REASON CODES ,Page 9-10)
<a class="top-link hide" href="#top" style="text-decoration:none">^</a>

<!-- ### Webhook Return Notification - Test Feature -->