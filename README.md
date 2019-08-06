# Wyre V3 API Collection for Postman

## Description

Hear, Hear. For those crypto developer enthusiasts looking to integrate with our back-end API, look no further. This repo is filled to the brim with sample Postman API requests that allow you to test our back-end endpoints.

## Environment

To get started, download this repo and upload the `Wyre V3 API Collection.postman_collections.json` file to Postman.

![import_collections](https://media.giphy.com/media/5z5Z9IxUP5QeMtMK9Z/giphy.gif)

### Environment Variables

You'll need to set up environment variables to use the Postman requests. If you don't know how to set environment variables, go ahead and give this a [read](https://blog.getpostman.com/2014/02/20/using-variables-inside-postman-and-collection-runner/).

Below are the environment variable names you'll need to enter to use the Postman scripts. The variable name are case sensitive.

| **Environment Variable Name** | **Environment Variable Value**             | **Explanation**                                                                    |
| ----------------------------- | ------------------------------------------ | ---------------------------------------------------------------------------------- |
| WYRE_APIKEY                   | Your Api Key                               | [Authentication](https://docs.sendwyre.com/docs/authentication)                    |
| WYRE_TOKEN                    | Your Secret Token                          | [Authentication](https://docs.sendwyre.com/docs/authentication)                    |
| ACCOUNT_ID                    | Your Account ID                            |
| publicToken                   | Your public token (Insert an Empty String) | [Connect a Bank Account](https://docs.sendwyre.com/docs/create-ach-payment-method) |

No need to fill those environment variables unless you already have an account with secret keys and api keys. If you don't have an account, you can read [this](https://support.sendwyre.com/en/articles/2855914-how-to-sign-up-for-wyre-as-a-partner) or you can generate secret/api keys from the key management folder as described in the Authentication paragraph of this repo.

## Getting Started

There are five folders in the Wyre V3 API Collection. Key Management, Account Overview, Payment Method Overview, Transfers and Exchanges and Data Subscriptions. Each one of these folders contains actions that encompass specific functionality. To get a better understanding of the postman script, you'll need to take a look at each Pre-req script in each file. Each Pre-req script gets and sets specific environment variables as well as calculates a [Secret Key Signature Authentication](https://docs.sendwyre.com/docs/authentication#secret-key-signature-auth). When doing post requests, if you change any data in the body, you have to make sure to change the data in the pre-req script, white spaces and all. It helps to justs copy/paste from the body to the pre-req script.

## Authentication

For those that already have a Wyre account and know there secret key and API key, read below. This postman collection uses secret key [signature authentication](https://docs.sendwyre.com/docs/authentication#secret-key-signature-auth). You won't be able to proceed unless you have:

- A Wyre Test Account with an account id
- An API Key
- A Secret Key

If you don't have a secret key or API key, you can create them within the postman collection in the Key Management folder described below. This flow will mimic how a user goes though the flow on your application.

### Key Management

The **Key Management** folder contains all the endpoints to create/delete API keys. If you do not have an account with credentials, you would first hit the POST Submit Auth Token. In the pre-req script, it creates a new token, sets the value of that to the WYRE_TOKEN env variable. The Wyre API will return an API Key. In the Tests tab in the postman collection, the API key is set to the env variable WYRE_APIKEY. After hitting the SEND button on this file, you are now authenticated with a secret token and an API key. You can now access other endpoints in the Account Overview, Payment Method Overview and Transfers Folder.

| **File Name**          | **Description**                                                                                                                      | **Resource**                                                            |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| POST Submit Auth Token | Creates a random token for the user in the pre-req script, submits that token to authenticate as the user, API returns back API keys | [Submit Key Auth](https://docs.sendwyre.com/docs/initialize-auth-token) |
| POST Create An API Key | Generates a new set of API credentials                                                                                               | [Create API Key](https://docs.sendwyre.com/docs/create-api-key)         |
| DELETE delete API Key  | Deletes an API Key                                                                                                                   | [Delete API Key](https://docs.sendwyre.com/docs/delete-api-key)         |

#### Account Overview

The **Account Overview** folder contains actions that get account information, create accounts, update accounts, and upload documents. In order to create an account, fill in the information in the body tab and make sure to copy that object to the pre-req script for the auth calculation.

| **File Name**        | **Description**                                                                                                                              | **Resource**                                                         |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| GET get_account      | Gets a users account information                                                                                                             | [Get Account](https://docs.sendwyre.com/docs/get-account)            |
| POST create_account  | Creates a new account                                                                                                                        | [Create Account](https://docs.sendwyre.com/docs/create-account)      |
| POST update_account  | Updates an Account, Make sure you check the pre-req script and the body tab and update the object with correct values to update the account. | [Update Account](https://docs.sendwyre.com/docs/submit-account-info) |
| POST upload_document | Upload document (Currently under development)                                                                                                | [Upload Document](https://docs.sendwyre.com/docs/upload-document)    |

#### Payment Method Overview

The **Payment Method Overview** folder contains actions that create a payment method, get payment method by id, get all payment methods from a user, delete a payment method, and attached a blockchain address to a payment method.

Before you are able to create a payment method, you'll need to follow these steps [here](https://docs.sendwyre.com/docs/create-ach-payment-method) and retrieve a publicToken. You will not be able to proceed with any of the functionality of the Payment Method Overview folder until you retrieve a publicToken. After you get the public token, make sure to set the publicToken as an environment variable in Postman manually. Now you can run the ACH Create Payment Method file. That will create a paymentId variable and set it as an environment variable for your Postman environment (In the Tests scripts in the ACH Create Payment Method File). Then you can run the GET payment_id file to get your payment method information.

| **File Name**                                | **Description**                                                                                                                                                                                                                                                                                                              | **Resource**                                                                                    |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| ACH Create Payment Method                    | Creates a new ACH Payment Method                                                                                                                                                                                                                                                                                             | [Create ACH Payment](https://docs.sendwyre.com/docs/local_transfer-ach-create-payment-method)   |
| POST ACH Create Payment Method Wire Transfer | Creates a Wire Transfer payment method                                                                                                                                                                                                                                                                                       | [Create Wire Transfer Payment Method](https://docs.sendwyre.com/docs/create-payment-method)     |
| GET payment_id                               | Gets a payment information with an id                                                                                                                                                                                                                                                                                        | [Get Payment Information](https://docs.sendwyre.com/docs/get-payment-method)                    |
| GET payment_methods                          | Gets all payment methods with the account                                                                                                                                                                                                                                                                                    | [Get Payment Methods](https://docs.sendwyre.com/docs/list-payment-methods)                      |
| DELETE payment_method                        | Deletes a payment method with an id. You'll need to manually set the paymentMethodId path variable in Postman and in the Postman Pre-req script. If you do not, you'll get a signature mismatch error. Run the GET payment_methods file to get a list of all the payment methods.                                            | [Delete Payment Method](https://docs.sendwyre.com/docs/delete-payment-method)                   |
| POST Attach Blockchain Address to Payment    | Attaches an address to a Payment Method. The file uses the paymentId created by the ACH Create Payment Method File to attach a blockchain address thats created by Wyre to attach to that paymentId. You can change the address to be ETH, BTC, or ALL. Make sure to update in the pre-script and body if you make a change. | [Attach Blockchain Address](https://docs.sendwyre.com/docs/attach-blockchain-to-payment-method) |

#### Transfers and Exchanges

The **Transfers and Exchanges** folder contains actions that create a transfer, get transfer information, confirm a transfer, get all transfer history and get the current exchange rates. When creating a transfer, you have the option to setting the transfer to autoConfirm. Read more [here](https://docs.sendwyre.com/docs/transfer-resources). The way the POST create_transfer file is currently implemented is without the autoConfirm key. The POST create_transfer script, after completed, sets a Postman transferId environment variable to your environment. After you run the POST create_transfer Postman script, you'll need to run the POST confirm_transfer script to confirm the transfer. You can alternatively set the key/value pair of autoConfirm:true in the create_transfers endpoint to auto confirm the transfer.

| **File Name**             | **Description**                                    | **Resource**                                                             |
| ------------------------- | -------------------------------------------------- | ------------------------------------------------------------------------ |
| POST create_transfer      | Create a new transfer for the user                 | [Create Transfer](https://docs.sendwyre.com/docs/create-transfer)        |
| GET transfer              | Gets information about the transfer based on id    | [Get Transfer](https://docs.sendwyre.com/docs/get-transfer)              |
| POST confirm_transfer     | Confirms a transfer if not set to autoConfirm:true | [Confirm Transfer](https://docs.sendwyre.com/docs/confirm-transfer)      |
| POST create wire transfer | Creates a wire transfer                            | [Create Transfer](https://docs.sendwyre.com/docs/create-transfer)        |
| GET transfer_history      | Gets transfer history from the user                | [Gets All Transfers](https://docs.sendwyre.com/docs/transfer-history)    |
| GET exchange_rates        | Gets current exchange rates for our API            | [Get Exchange Rates](https://docs.sendwyre.com/docs/live-exchange-rates) |

#### Data Subscriptions (Web hooks)

The **Data Subscriptions** folder contains actions to subscribe to a webhook, delete a webhook and get all subscription information. Take a look at the body and the Postman pre-req script to set your body params. You'll need to set the subscribeTo and notifyTarget key to set up the webhooks. After executing the POST subscribe_to_a_webhook file, the Postman test tab sets a webhookId environment variable that is then used in the DELETE webhook file. You'll need to set a new webhookId to delete if you want to delete a different webhook id.

| **File Name**                    | **Description**                            | **Resource**                                                          |
| -------------------------------- | ------------------------------------------ | --------------------------------------------------------------------- |
| POST subscribe_to_a_webhook      | Subscribe to a webhook for an SRN resource | [Subscribe Webhook](https://docs.sendwyre.com/docs/subscribe-webhook) |
| DELETE webhook                   | Deletes a webhook by an id                 | [Delete Webhook](https://docs.sendwyre.com/docs/delete-subscription)  |
| GET all_subscription_information | Gets all the current subscribed resources  | [Get All Webhooks](https://docs.sendwyre.com/docs/get-subscriptions)  |
