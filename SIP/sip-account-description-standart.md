---
sip: <to be assigned>
title: <Signum Account Description Standart>
description: <Single decentralized truth source in account implemented by using an accounts description>
author: <Shefas (@shefass)>
discussions-to: <URL>
status: Draft
type: <Informational>
created: <2022-02-21>
---

## Abstract
Single decentralized truth source in account implemented by using an accounts description. This is a standard that can be followed by developers to have consistency between different applications.

## Motivation
An account can hold arbitrary data which can be used. This data must follow a standard, so it was easy to use, create, edit by different parties. 

## Specification
JavaScript Object Notation (JSON) MUST be used for data encoding. 
The first element MUST be a "vr" (version) with a number. It represents the version of a schema used for data encoding. 
Versions can be created freely if the last versions are not useful in a specific project and put in this SIP. 

Version 1. (here goes signumart scheme).

## Rationale
Every account in Signum is represented by an account address (3 formats) or an alias. An account can have a name (maximum 100 characters) and/or description (maximum 1000 characters). 

Account description is used for data holding because account description can hold 1000 characters.

## Reference Implementation
(Link to version 1 schema)

## Security Considerations
Encoded data can link to malicious code, so it must be checked before usage. 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
