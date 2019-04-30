# Wyre V3 API Collection for Postman

## Description

Hear, Hear. For those crypto developer enthusiasts looking to integrate with our back-end API, look no further. This repo is filled to the brim with sample Postman API requests that allow you to test our back-end endpoints.

## Environment

To get started, download this repo and upload the `Wyre V3 API Collection.postman_collections.json` file to Postman.

![import_collections](https://media.giphy.com/media/5z5Z9IxUP5QeMtMK9Z/giphy.gif)

### Create a Wyre Account

In order to access our API, you'll first need to create a Wyre account. If you don't already have a Wyre test account, follow this [tutorial](https://support.sendwyre.com/getting-started/how-to-sign-up-for-wyre-as-a-partner) and create a Wyre test account. Make sure you generate API/Secret Keys, you'll need them in order to authenticate to use our backend API endpoints. You won't be able to proceed unless you have:

- A Wyre Test Account with an account id
- An API Key
- A Secret Key

### Environment Variables

Once you have those three items, you'll need to set environment variables to use the Postman requests. If you don't know how to set environment variables, go ahead and give this a [read](https://blog.getpostman.com/2014/02/20/using-variables-inside-postman-and-collection-runner/).

Below are the environment variable names you'll need to enter to use the Postman scripts. The variable name are case sensitive.

| **Environment Variable Name** | **Environment Variable Value**             | **Explanation**                                                                    |
| ----------------------------- | ------------------------------------------ | ---------------------------------------------------------------------------------- |
| WYRE_APIKEY                   | Your Api Key                               | [Authentication](https://docs.sendwyre.com/docs/authentication)                    |
| WYRE_TOKEN                    | Your Secret Token                          | [Authentication](https://docs.sendwyre.com/docs/authentication)                    |
| ACCOUNT_ID                    | Your Account ID                            |
| publicToken                   | Your public token (Insert an Empty String) | [Connect a Bank Account](https://docs.sendwyre.com/docs/create-ach-payment-method) |

## Getting Started

There are four folders in the Wyre V3 API Collection. Account Overview, Payment Method Overview, Transfers and Exchanges and Data Subscriptions. Each one of these folders contains actions that encompass specific functionality. To get a better understanding of the postman script, you'll need to take a look at each Pre-req script in each file. Each Pre-req script gets and sets specific environment variables as well as calculates a [Secret Key Signature Authentication](https://docs.sendwyre.com/docs/authentication#secret-key-signature-auth).

#### Account Overview

The **Account Overview** folder contains actions that get account information, create accounts, update accounts, and upload documents.

| **File Name**        | **Description**                                                                                                                              | **Resource**                                                         |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| GET get_account      | Gets a users account information                                                                                                             | [Get Account](https://docs.sendwyre.com/docs/get-account)            |
| POST create_account  | Creates a new account                                                                                                                        | [Create Account](https://docs.sendwyre.com/docs/create-account)      |
| POST update_account  | Updates an Account, Make sure you check the pre-req script and the body tab and update the object with correct values to update the account. | [Update Account](https://docs.sendwyre.com/docs/submit-account-info) |
| POST upload_document | Upload document (Currently under development)                                                                                                | [Upload Document](https://docs.sendwyre.com/docs/upload-document)    |

#### Payment Method Overview

The **Payment Method Overview** folder contains actions that create a payment method, get payment method by id, get all payment methods from a user, delete a payment method, and attached a blockchain address to a payment method.

Before you are able to create a payment method, you'll need to follow these steps [here](https://docs.sendwyre.com/docs/create-ach-payment-method) and retrieve a publicToken. You will not be able to proceed with any of the functionality of the Payment Method Overview folder until you retrieve a publicToken. After you get the public token, make sure to set the publicToken as an environment variable in Postman manually. Now you can run the ACH Create Payment Method file. That will create a paymentId variable and set it as an environment variable for your Postman environment (In the Tests scripts in the ACH Create Payment Method File). Then you can run the GET payment_id file to get your payment method information.

| **File Name**                             | **Description**                                                                                                                                                                                                                                                                                                              | **Resource**                                                                                    |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| ACH Create Payment Method                 | Creates a new ACH Payment Method                                                                                                                                                                                                                                                                                             | [Create ACH Payment](https://docs.sendwyre.com/docs/local_transfer-ach-create-payment-method)   |
| GET payment_id                            | Gets a payment information with an id                                                                                                                                                                                                                                                                                        | [Get Payment Information](https://docs.sendwyre.com/docs/get-payment-method)                    |
| GET payment_methods                       | Gets all payment methods with the account                                                                                                                                                                                                                                                                                    | [Get Payment Methods](https://docs.sendwyre.com/docs/list-payment-methods)                      |
| DELETE payment_method                     | Deletes a payment method with an id. You'll need to manually set the paymentMethodId path variable in Postman and in the Postman Pre-req script. If you do not, you'll get a signature mismatch error. Run the GET payment_methods file to get a list of all the payment methods.                                            | [Delete Payment Method](https://docs.sendwyre.com/docs/delete-payment-method)                   |
| POST Attach Blockchain Address to Payment | Attaches an address to a Payment Method. The file uses the paymentId created by the ACH Create Payment Method File to attach a blockchain address thats created by Wyre to attach to that paymentId. You can change the address to be ETH, BTC, or ALL. Make sure to update in the pre-script and body if you make a change. | [Attach Blockchain Address](https://docs.sendwyre.com/docs/attach-blockchain-to-payment-method) |

#### Transfers and Exchanges

The **Transfers and Exchanges** folder contains actions that create a transfer, get transfer information, confirm a transfer, get all transfer history and get the current exchange rates. When creating a transfer, you have the option to setting the transfer to autoConfirm. Read more [here](https://docs.sendwyre.com/docs/transfer-resources). The way the POST create_transfer file is currently implemented is without the autoConfirm key. The POST create_transfer script, after completed, sets a Postman transferId environment variable to your environment. After you run the POST create_transfer Postman script, you'll need to run the POST confirm_transfer script to confirm the transfer. You can alternatively set the key/value pair of autoConfirm:true in the create_transfers endpoint to auto confirm the transfer.

| **File Name**         | **Description**                                    | **Resource**                                                             |
| --------------------- | -------------------------------------------------- | ------------------------------------------------------------------------ |
| POST create_transfer  | Create a new transfer for the user                 | [Create Transfer](https://docs.sendwyre.com/docs/create-transfer)        |
| GET transfer          | Gets information about the transfer based on id    | [Get Transfer](https://docs.sendwyre.com/docs/get-transfer)              |
| POST confirm_transfer | Confirms a transfer if not set to autoConfirm:true | [Confirm Transfer](https://docs.sendwyre.com/docs/confirm-transfer)      |
| GET transfer_history  | Gets transfer history from the user                | [Gets All Transfers](https://docs.sendwyre.com/docs/transfer-history)    |
| GET exchange_rates    | Gets current exchange rates for our API            | [Get Exchange Rates](https://docs.sendwyre.com/docs/live-exchange-rates) |

#### Data Subscriptions (Web hooks)

The **Data Subscriptions** folder contains actions to subscribe to a webhook, delete a webhook and get all subscription information. Take a look at the body and the Postman pre-req script to set your body params. You'll need to set the subscribeTo and notifyTarget key to set up the webhooks. After executing the POST subscribe_to_a_webhook file, the Postman test tab sets a webhookId environment variable that is then used in the DELETE webhook file. You'll need to set a new webhookId to delete if you want to delete a different webhook id.

| **File Name**                    | **Description**                            | **Resource**                                                          |
| -------------------------------- | ------------------------------------------ | --------------------------------------------------------------------- |
| POST subscribe_to_a_webhook      | Subscribe to a webhook for an SRN resource | [Subscribe Webhook](https://docs.sendwyre.com/docs/subscribe-webhook) |
| DELETE webhook                   | Deletes a webhook by an id                 | [Delete Webhook](https://docs.sendwyre.com/docs/delete-subscription)  |
| GET all_subscription_information | Gets all the current subscribed resources  | [Get All Webhooks](https://docs.sendwyre.com/docs/get-subscriptions)  |
