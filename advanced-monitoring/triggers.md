---
description: Customize triggers to fit your security requirements
---

# Triggers

## Event Emitted

This trigger alerts you whenever an event is emitted from your smart contract, giving you real-time visibility into what's happening.

**Trigger type parameters:**&#x20;

* **Event:** execution of certain actions within the contract

## **Function Call**

This trigger alerts you whenever a function is executed on your smart contract.

**Trigger type parameters:**&#x20;

* **Function:** a method defined in the contract

## ERC20 Transfer

This trigger alerts you whenever an ERC20 token transfer of more than a specified amount occurs, allowing you to monitor large transfers and protect against potential theft.

**Trigger type parameters:**&#x20;

* **Token address:** an address of the token that must be monitored
* **Amount:** token amount as a limit, transfer that exceeds the limit will be noticed
* **Decimals:** specifies the smallest fraction of a token that can be handled
* **Destination:** from, to or any of the contract

## Blacklisted Callers

This trigger alerts you if a transaction is initiated from a blacklisted address, helping you to be informed of suspicious activity from known problematic accounts or contracts.

**Trigger type parameters:**&#x20;

* **Blacklisted Addresses:** a list of addresses

## Whitelisted Callers

This trigger alerts you if a transaction is initiated from an address that is not on your whitelist, helping you to be informed of unauthorized access to your contract.

**Trigger type parameters:**&#x20;

* **Whitelisted Addresses:** a list of addresses

## Failed Transaction

This trigger alerts you if a transaction fails, allowing you to quickly respond to any issues and minimize potential damage.

**Trigger type parameters:** None
