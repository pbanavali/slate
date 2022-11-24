---
title: Kapcharge API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL

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
  - <a href='https://www.kapital.solutions/terms'>Terms and Conditions</a> | <a href='https://www.kapital.solutions/privacy'>Privacy Policy</a>
----
# Introduction

This technical integration specification document explains the KAPCHARGE Payment Gateway new API. It enables merchant systems to post real-time transaction requests to KAPCHARGE Payment Gateway.

This part of the documentation discusses the technical requirements for real-time integration only. For specifications relating to Batch File Uploading or using Virtual Terminals to create and send the transactions online, please refer to the other services of the Kapcharge.

# <span class="kapcharge-highlight">Kapcharge</span> Payment Gateway

Description| Transaction Types
--------- | -----------
**CHARGE - Receivable**| Allows the transfer of funds from a customer's bank account to your merchant account. This transaction is completed in real time.
**CREDIT - Payable** | Allows the transfer of funds from a merchant account directly to a customer's bank account. This transaction is completed in real time.

# API Specification

## Environments
Staging:

* Application Url: https://api-staging.kapcharge.com/
* Access Token Url: https://kapitalincauth.b2clogin.com/kapitalincauth.onmicrosoft.com
* Authorization Scope: https://kapitalincauth.onmicrosoft.com/109bc12d-3fc6-4a18-881b-bbc059da857e/.default

Production:

*   Please contact your Account Manager to obtain.

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

[![Run in Postman](https://run.pstmn.io/button.svg)](https://god.gw.postman.com/run-collection/18538588-9d0a6b1c-fc9c-4d71-b34c-b9ddc9d6f682?action=collection%2Ffork&collection-url=entityId%3D18538588-9d0a6b1c-fc9c-4d71-b34c-b9ddc9d6f682%26entityType%3Dcollection%26workspaceId%3Dbceb4dfb-9362-4a88-9174-f6595e89dc8c)

<aside class="notice">
Run in Postman button automatically stay updated with changes in the Kapcharge original API collection, so you always get the most recent version of Kapcharge API collection without having to manually update the collection's link.
</aside>

## <span class="kapcharge-highlight">Kapcharge</span> Swagger API 
 <a href="https://api-staging.kapcharge.com/swagger/index.html" class="kapcharge-btn">Swagger API</a>

## Authentication

### OAuth GET AccessToken
Bearer Token Authorization is used for all API requests. Multiple API calls can be made securely without requiring authorisation each time with a Bearer Token.

### Payload

> Authentication Payload

```shell
{
  grant_type:client_credentials
  scope:{{Authorization Scope}}
  client_id:{{ClientID}}
  client_secret:{{ClientSecret}}
}
```
### HTTP Request
`Method: POST`
`Endpoint: {{Access Token Url}}/B2C_1_su_si_1/oauth2/v2.0/token`

### Transaction Request

Parameter | Type| Mandatory | Description
--------- | ------- |------- | -----------
grant_type |string| yes | This parameter should always be set to client_credentials.
scope |string| yes  | The AuthorizationScope variable in the Environment section should determine this value.
client_id |string| yes | Please contact your Account Manager to obtain.
client_secret |string| yes | Please contact your Account Manager to obtain.

> This API request must pass the header Content-type: application/x-www-form-urlencoded; unless explicitly mentioned.
> Make sure to replace `{{Access Token Url}}` with API Access Token Url as per the environment. 

```shell
curl --location -g --request POST '{{Access Token Url}}/B2C_1_su_si_1/oauth2/v2.0/token' \
```

<aside class="notice">
This API request must pass the header Content-type: application/x-www-form-urlencoded; unless explicitly mentioned.<br/>
You must replace <code>{{Access Token Url}}</code> with API Access Token Url as per the environment.
</aside>

> The above command returns JSON structured like this:

```json
{
    "access_token": "{JWT Token}",
    "token_type": "Bearer",
    "not_before": 1666294831,
    "expires_in": 3600,
    "expires_on": 1666298431,
    "resource": ""
}
```
### Responses

Parameter | Description
--------- | ------- 
access_token| The authorization token that needs to be used throughout the API request.
token_type| This will always be "Bearer" token type.
not_before| This is token validation start-time (epoch time).
expires_in| This is token expiration time (seconds).
expires_on| This is token validation end-time (epoch time).

## EFT Transactions

### Create Transactions
### Payload

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
`Method: POST`
`Endpoint: {{ Application Url }}/api/v1/transaction`

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
Customer|[object](#customer)|yes|  | Pass the Customer object with all the required details as seen in Customer below.
ShippingInformation|[object](#shipping-information)|no| | Pass the Shipping Information object with all the required details as seen in Shipping Information below.
CustomerBankAccount|[object](#customer-bank-account)|yes| | Pass the Customer Bank Account object with all the required details as seen in Customer Bank Account below.

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
IdentificationType |string|no|Identification Type of the customer, check Identification Type [here](#identification-type).
IdentificationNumber |string|no|Identification Number of the customer (maximum0 characters).
IdentificationCountry |string|no|Identification country.Refer notice below.
IdentificationProvince |string|no|Identification state/province.
IdentificationExpirationDay |string|no|Identification Expiration Day of the month.
IdentificationExpirationMonth |string|no|Identification Expiration Month of the year.
IdentificationExpirationYear |string|no|Identification Expiration Year.



```shell
curl --location -g --request POST '{{ Application Url }}/api/v1/transaction' \
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.
> For any parameter related to Country ,please use the <b>alpha-2 standard</b>.<br/>Follow the link below:<br/> 
> [ISO Country Codes](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes).

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
For any parameter related to Country ,please use the <b>alpha-2 standard</b>.<br/>Follow the link below:<br/> 
</aside>

[ISO Country Codes](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes)

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
AccountType |string|yes|Check Account Type [here](#account-type).
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
transactionStatus|Refer KAPCHARGE Bank Status Response Codes below table.
transactionFlow| Either "Debit" or "Credit".
amountInCents| Transaction amount in cents.
listOfErrors| Error message (only when there is some error processing request).

#### KAPCHARGE Transaction Status Codes

Description|Status Code
--------- | ----------- 
Accepted|0
InProgress |2
Rejected|5
CommunicationFailure|90
AuthenticationFailed|1001
ValidationCheckFailed |9001
SystemError|9000
Cancelled |-1
Declined |1

### Get a specific transaction

### HTTP Request
`Method: GET`
`Endpoint: {{ Application Url }}/api/v1/transaction/<TransactionID>`

```shell
curl --location -g --request GET '{{ Application Url }}/api/v1/transaction/12345679' \
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
</aside>

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

This endpoint retrieves a specific transaction request based on transaction id.

### Responses

Parameter | Description
--------- | ------- 
kapchargeTransactionId| The unique transaction ID generated by Kapcharge.
clientReference|Transaction reference provided by the client.
transactionStatus|Refer KAPCHARGE Bank Status Response Codes below table.
transactionFlow|Either "Debit" or "Credit".
amountInCents| Transaction amount in cents.


### Delete a specific transaction

### HTTP Request
`Method: DELETE`
`Endpoint: {{Application Url}}/api/v1/transaction/<TransactionID>`

```shell
curl --location -g --request DELETE '{{Application Url}}/api/v1/transaction/12345679' \
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
transactionStatus|Refer KAPCHARGE Bank Status Response Codes below table.
transactionFlow|Either "Debit" or "Credit".
amountInCents| Transaction amount in cents.

## Webhooks Configuration

### Get Webhooks Configuration for a specific merchant
The webhooks aim to give your organization a real-time update on the status of transactions.

To enable webhooks, you must send the below information through the API. When a URL is defined, Kapcharge will send a POST request each time there is a status change to "rejected".

### HTTP Request
`Method: GET`
`Endpoint: {{Application Url}}api/v1/configuration/<MerchantID>`

```shell
curl --location -g --request GET '{{Application Url}}/api/v1/configuration/TEST_123-KC' \
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
</aside>

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
`Method: PUT`
`Endpoint: {{Application Url}}/api/v1/configuration`

```shell
curl --location -g --request PUT '{{Application Url}}/api/v1/configuration'
```

> Make sure to replace `{{Application Url}}` with Application Url as per the environment.

<aside class="notice">
You must replace <code>{{Application Url}}</code> with Application Url as per the environment.<br/>
</aside>

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