---
title: FIO Requests
description: FIO Requests
redirect_from:
    - /docs/integration-guide/fio-request
---

# FIO Requests

A FIO Request is a transaction in which a payee is requesting funds from a payer using FIO Addresses.  The details of FIO Requests are private and only readable by the counter parties to the transaction.  

This request is stored on-chain, and all sensitive metadata is encrypted (currency, amount, public address of payee, FIO Data, etc.) using the Diffie-Hellman key method, which utilizes a shared secret from the payee private key and payer public key. This encryption is always enabled for all FIO Requests.

The payer's wallet or application polls the FIO Chain for relevant requests, and when found, decrypts the request inside their wallet (at which point a wallet can choose to implement a notification mechanism or FIO Request UI). This information and then used to pre-populate the send screen of the relevant blockchain whose token/coin is being requested.

The payer has a choice whether to approve or reject the request. If rejected, there is a status update made on FIO Chain, which removes the request from being shown. 

**FIO Requests are included as part of the bundled transaction with registering/renewing a FIO Address.**

FIO Request is a perfect mechanism for recurring payments, where a merchant can trigger a request anytime a payment is due. This request will show up in user’s wallet for approval.

In order to enable FIO Protocol on an e-commerce site, the merchant will need to simply collect the FIO Address from the customer on the site and initiate a FIO Request to that address for the required amount of cryptocurrency at checkout. The process of recognizing the payment would remain unchanged.

## FIO Requests and FIO Data

One of the key features of FIO Protocol is the ability to request crypto using FIO Request. The user requesting funds (Payee) can send a FIO Request to a user who is asked to pay (Payer), by only using the Payer’s FIO Address.

FIO Request data, such as amount, currency, and memo field are encrypted and only readable by Payee and Payer.

{% include alert.html type="info" title="FIO Request does not use mapped addresses"  content="The crypto public address specified in FIO Request does NOT have to be the same as public_address mapped using /add_pub_address" %}

Please read [Encrypting FIO Data]({{site.baseurl}}/docs/how-to/encryption) to better understand how encryption works.

### Submitting new FIO Request

To request funds, Payee’s wallet should submit a new FIO Request using [/new_funds_request]({{site.baseurl}}/pages/api/fio-api/#options-newfundsreq) API method.

### Fetching pending FIO Requests

The Payer’s wallet can fetch all new and pending FIO Requests using [/get_pending_fio_requests]({{site.baseurl}}/pages/api/fio-api/#post-/get_pending_fio_requests) API method.

### Fetching received FIO Requests

The Payer’s wallet can fetch all FIO Requests that have been received by the provided FIO public key using [/get_received_fio_requests]({{site.baseurl}}/pages/api/fio-api/#post-/get_received_fio_requests) API method.

### Rejecting a FIO Request

The Payer’s wallet can reject a FIO Request, which will remove it from the pending list, using [/reject_funds_request]({{site.baseurl}}/pages/api/fio-api/#options-rejectfndreq) API Method.

### Fetching sent FIO Requests

The Payee’s wallet can fetch all sent FIO Requests and its current status using [/get_sent_fio_requests]({{site.baseurl}}/pages/api/fio-api/#post-/get_sent_fio_requests) API method.

### Recording FIO Data

Anytime crypto is sent using FIO Address, optional metadata such as amount, currency, and memo, may be recorded on the FIO Chain. FIO Data is encrypted and only readable by Payee and Payer.

See [How to Record and Retrieve FIO Data]({{site.baseurl}}/docs/how-to/fio-data) for more information on using FIO data.

### Example flow

#### Alice requests 1 BTC from Bob and adds a “Invoice 123” memo

* BTC public address, amount, memo and other data [are encrypted]({{site.baseurl}}/docs/how-to/encryption)
* [/new_funds_request]({{site.baseurl}}/pages/api/fio-api/#options-newfundsreq) is submitted to FIO Chain.
* /get_sent_fio_requests will return the request just sent with the status requested and all encrypted data including “Invoice 123” memo. We recommend wallets show this request with a status of “pending”.
* /get_pending_fio_requests will not return anything as this request was for Bob, not for Alice.

#### Bob checks his wallet for new requests

* /get_sent_fio_requests will not return anything as this request was sent by Alice not by Bob.
* /get_pending_fio_requests will return the request just sent with the status requested and all encrypted data including “Invoice 123” memo. We recommend wallets show this request for Bob to act on.

#### Bob accepts the request and pays Alice

##### Step 1

* BTC public address, amount and memo [are decrypted]({{site.baseurl}}/docs/how-to/encryption)
* Wallet creates a payment for 1 BTC to provided BTC public address and “Invoice 123” memo.
* User has the option to modify amount or memo.
* Once user approves, the transaction is broadcasted to Bitcoin blockchain.

##### Step 2

* Actual amount, actual memo, transaction ID (obt_id) from Bitcoin blockchain and other data are encrypted
* [/record_obt_data]({{site.baseurl}}/pages/api/fio-api/#options-recordobt) is sent to FIO Chain

#### Alice checks payment

* /get_sent_fio_requests will return the request with the status sent_to_blockchain. We recommend wallets show this request as “received”.
* [/get_obt_data]({{site.baseurl}}/pages/api/fio-api/#post-/get_obt_data) will return important [encrypted]({{site.baseurl}}/docs/how-to/encryption) metadata wallets should attach to the request and/or the actual Bitcoin transaction including actual amount, actual memo, transaction ID (obt_id) from Bitcoin blockchain and other data. obt_id may be used to match the information with the actual Bitcoin blockchain transaction.

#### In Bob’s wallet

* /get_sent_fio_requests will not return anything as Bob never sent a request.
* /get_pending_fio_requests will not return anything as Bob already responded to Alice’s request.

