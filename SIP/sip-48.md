---
sip:  48
title: Alias renewal process
description: Introducing a new ownership handling for aliases on a yearly basis
author:   frank_the_tank
status: Review
type: standard
category: Core
created: 2022-12-10
---
## Abstract
This SIP will introduce a temporary ownership of aliases which can be prolonged on a yearly basis.

## Motivation
Signum offers a simple setup to create aliases for an account. Once set/created those aliases are owned by the creator account forever. With the introducting of [SIP-44](sip-44.md)  the alias will be more powerful to use for like domain, account or application pointer which makes a reserved alias name more valuable. In addition with [SIP-47](sip-44.md) a new service SNS (Signum Naming Service) will be introduced which is a distributed, open, and extensible naming system based on the Signum blockchain. Given these cases, those aliases should be actively maintained by the creator/owner to flag those aliases are in use by them, otherwise another user should be able to claim the alias.

## Specification
The following changes should be done with an upcoming hard-fork to introduce the renewal process for aliases.

### Database changes
On the database level, the alias table will get a new field `renewal_time` which is an INT field and contains the timestamp until the alias is reserved for the current owner. With introduction of the needed hard-fork all aliases will get a reserved datetime of 3 months after the planned hard fork. In this case all active users have enough time to check their current alias ownerships and can create a renewal subscription if desired.

### Node changes
**Transaction Set Alias**

If a user sends a setAlias transaction the following datetime handling for the alias will be executed:
 - If the creation is before the block of the hard fork no datetime is set on `renewal_time`.
 - If the creation is after the hard fork and no datetime is set for the alias or a datetime which is in the past or shorter than 24 hours in the future, a new datetime will be set with datetime plus 24 hours on `renewal_time`.
 - If the creation is after the hard fork but  the datetime is longer than 24 hours in the future no datetime adjustment will happen on `renewal_time`
 - If the setAlias leads to a new alias the `renewal_time` is set with a value of 24 hours in the future.

In addition, if the setAlias transaction is executed for an existing alias and the creator of the setAlias transaction is not the current owner, the transaction - which leads to an automated owner transfer -  will only be executed if the renewal timestamp of the alias has already expired.

**Auto renewal for aliases**

Signum has already all the basic functionality to empower the user for an auto-renewal via the existing subscription transaction. A subscription is valid for the auto-renewal when the following parameters are given:

- The subscription is created with an attachment. A new field `alias` need to be set to bind the subscription to a given alias.
- The subscription amount is paid to a defined receiver account.
- The subscription should have minimum amount of 70 SIGNA as payment.
- The subscription should be set to a 1 year interval (in seconds).

**New node logic by execution of a subscription:**

By creation or an interval payment of the subscription the node will check the above parameters. If the creator of the subscription is still the owner of the alias and the balance of the account sufficient to execute the subscription, the alias will get an update on the `renwal_time` table field by one year and 24 hours (based on the execution time of the subscription). If the creator is no longer the owner of the alias the subscription will be cancelled before any further execution.

**API support**

The auto-renewal can be created with the current create subscription transaction, but to make it more seamless for the user and developer a new API call should be introduced with the following fix parameters:

- **receiver** is equal the fixed receiver account
- **frequency** is equal 1 year in seconds
- **amount** is equal 70 Signa
- **alias** should be set with the aliasId to renewal - with this attribute the attachment will be created.

The transaction created with this call is still the same as the standard subscription creation.


###  Renewal costs
The renewal costs are set with 70 Signa. The reason for this amount is given by the alternative that a user needs to execute setAlias every day to update the `reneval_time` by 24 hours. This would generate costs of 365 x 0.2 Signa = 73 Signa. 

### Multiverse support
To be able to have later mulitverse support for this feature we should have a config entry for the receiver address to be a valid auto-renewal subscription.

## Backwards Compatibility  
This is a hard forking change, thus breaking compatibility with old fully-validating nodes. It should not be deployed without widespread consensus. 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).