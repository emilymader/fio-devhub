---
title: FIO Addresses & Domains
description: FIO Addresses & Domains
---

# FIO Addresses & Domains

## FIO Addresses

FIO Addresses act as the human-readable wallet identifier for users on the FIO Protocol. They are necessary for users to utilize all the capabilities of the FIO Protocol, but also serve as a replacement for complicated public addresses across all tokens and coins in the user's wallet.

FIO Addresses have the construct of username@domain.  Examples of a FIO Address include:

**john@edgewallet**

or

**alice@crypto**

Registration of a FIO Address is done directly through a FIO-enabled application, or through a hosted registration site that is open to the community.

The FIO Address itself and all subsequent interactions with the FIO Protocol are controlled via a FIO private key. Loss of access to this private key suffers the same consequences as with any crypto asset - the FIO Address will no longer be usable by the original owner, nor can it be retrieved.

### Format of a FIO Address

A FIO Address consists of a username and a domain delimited by an @ symbol, there are not specific requirements around a FIO Address username, but there are format restrictions around the FIO Address as a whole, and then separate requirements for a FIO Domain in particular.

| |FIO Address (username@domain) |FIO Domain |
|---|---|---|
|Min. Chars | 3 | 1 |
|Max Chars | 64 (including FIO Domain) | 62 |
|Characters allowed	|ASCII a-z 0-9 - (dash) with single @ (at sign) |Domains may contain ASCII a-z 0-9 - (dash) |

** *The dash can not be the first or last character of the username or domain* 

#### FIO Address validation using regex

Regex validation may be used. For example, the following regex may be used to validate a FIO Address: 

`^(?:(?=.{3,64}$)[a-zA-Z0-9]{1}(?:(?!-{2,}))[a-zA-Z0-9-]*(?:(?<!-))@[a-zA-Z0-9]{1}(?:(?!-{2,}))[a-zA-Z0-9-]*(?:(?<!-))$)`

### Mapping a FIO Address to crypto public addresses

See [Mapping Public Addresses](/docs/how-to/mapping) in the Integration Guide.

## FIO Domains

FIO Addresses consist of a username and a domain. While having a FIO Address is necessary to use the FIO Protocol, users do not need to own their own FIO Domain. By default, many applications (especially wallets and exchanges) have a domain available as a default for their users. For example, the mobile wallet, Edge Wallet, has @edge domain as a default for their users.  Users which desire their own unique FIO domain may register one that is available as a non-fungible token that they control and own.  

Outside of centralized services (which own all user private keys, including FIO Addresses), **users are not obligated to use a specific domain** for their FIO Address, regardless of the wallet they choose to use.

### Domain Owner Abilities

A domain owner has several unique actions that are specific towards their management of a domain and any subsequent registrations of FIO Addresses.

* Public/Private Flag - by default, a FIO Domain can only be used for a FIO Address registration from the domain owner themselves. If chosen, the domain owner can change their domain to public, which would allow anyone with a FIO private key to register a FIO Address on their domain. This functionality is inherent to the mechanics of the FIO Protocol, though whether a particular wallet provides that option through their UI may differ.
* Expire Domain - a FIO Domain owner has the ability, at their discretion, to expire their domain. When they do, the domain enters expired state. 

Importantly - a domain owner is NOT able to control FIO Addresses on their domain, decrypt any data associated with transactions made by any registered FIO Addresses on their domain, nor are they able to change the fee structure of FIO Address registration/renewal as that is dictated by the block producers (this is separate from running a registration site, which allows for various other economic choices).

### Storing, Transferring, and Selling FIO Address Domains

All FIO Domains are non-fungible tokens, which means they can be freely transferred between accounts, users, and wallets as with any other crypto asset.

Future functionality will enable selling a FIO Domain through the use of a built-in escrow smart contract, in which both the domain owner and the buyer deposits their respective NFT and payment before transfer of the domain to the new owner is executed.

### FIO Domains and Decentralized Domain Efforts

A number of projects are underway with the goal of creating a decentralized domain name space system for the World Wide Web.  Should any of those projects become recognized in the community as the primary decentralized domain space, then the Foundation's vision is that the FIO Protocol will hopefully endeavor to work collaboratively with them, though such a decision will be up to the protocol Block Producers.  Specifically, FIO domains do not currently have the concept of a Top Level Domain (e.g., .com, .org, .net, .io) but such a construct could be added in the future to the notion of a FIO Address and a unique TLD for FIO Addresses registered for the FIO Protocol enabling FIO Addresses to work seamlessly with a decentralized domain name space. 

## Domain/Address Expiry

How to Renew Your FIO Domains and FIO Addresses
You can view complete instructions including screenshots for how to renew your FIO Domains and FIO Addresses with Edge Wallet, the FIO Registration Helper, the fio.bloks.io Block Explorer, or the API directly here: [How to Renew Your FIO Address or FIO Domain](https://peakd.com/fio/@fioprotocol/how-to-renew-your-fio-address-or-fio-domain){:rel="nofollow noopener noreferrer" target="_blank"}.

### FIO Domain

If the renewal fee for a FIO Domain is not paid by its expiration date, the following restrictions will be placed on the domain and addresses on that domain:

|Days after expiration date |Domain actions disallowed |Address actions disallowed |
|---|---|---|---
|0 - 30 | set_fio_domain_public | register_fio_address|
|31 - 90 |set_fio_domain_public |register_fio_address <br> renew_fio_address <br> add_pub_address <br> new_funds_request (Payee FIO Address only) <br> reject_funds_request (Payer FIO Address only) <br> record_send (Payer FIO Address only) <br> register_producer <br> register_proxy <br> proxy_vote <br> claim_bp_rewards |
|On day 90 |Domain is burned and can be re-registered by any user. |All addresses on that domain and associated data are burned. |

It's important to note that anyone can renew a domain as long as they are willing to pay the renewal fee. This ensures that users with FIO Addresses on abandoned domain, can continue to use it. 

### FIO Address

If the renewal fee for a FIO Address is not paid by its expiration date, the following actions are disallowed for 365 days:
* add_pub_address
* new_funds_request (Payee FIO Address only)
* reject_funds_request (Payer FIO Address only)
* record_send (Payer FIO Address only)
* register_producer
* register_proxy
* proxy_vote
* claim_bp_rewards

After 365 days the FIO Address is burned and can be re-registered by any user.
