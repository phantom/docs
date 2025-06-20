# Solana Priority Fees

### Introduction

Phantom automatically calculates and applies [Priority Fees](https://docs.solana.com/proposals/fee_transaction_priority) to all Phantom-generated transactions and dApp-generated transactions that meet [our requirements](solana-priority-fees.md#how-phantom-applies-priority-fees-to-dapp-transactions).

### How Transaction Fees Work on Solana

Solana transactions fees are calculated based on two main parts:

• A statically set base fee per signature, and

• The computational resources used during the transaction, measured in **Compute Units** **(CU)**

Since each transaction requires different computational resources, they are allotted a maximum number of compute units, known as the Compute Budget. After the Compute Budget is exhausted, the runtime halts the transaction and returns an error, resulting in a failed transaction.

The maximum budget for a transaction is 1.4 million CU, while the total blockspace limit is 48 million CU. Only a few computationally heavy TXs, like a mint or a swap, could fill the entire block, halting other TXs—Hence the need for **Priority Fees**.

The fee priority of a transaction `T` can be defined as `F(T)`, where `F(T)` is the _"fee-per compute-unit"_, calculated by:

$$
(additional\_fee + base\_fee)\  /\  requested\_compute\_units
$$

This means that the more compute units a transaction requests, the more additional fee it will have to pay to maintain the priority in the transaction queue. This prevents computationally heavy transactions from being easily spammed or from filling blocks.

### Priority Fees Calculation

Priority fees are calculated as,

$$
priority\_fees\ =\ compute\_budget\ *\ compute\_unit\_price
$$

where,

$$
compute\_budget\ =\ \#\ of\ instructions\ *\ compute\_unit\_limit
$$

### How Phantom Applies Priority Fees to dApp Transactions

Although dApps can set their own [priority fees on transactions](https://solanacookbook.com/references/basic-transactions.html#how-to-change-compute-budget-fee-priority-for-a-transaction) they generate, we highly discourage doing so as it often surfaces unnecessary complexity to end-users. Instead, we recommend that dApp developers let Phantom apply priority fees on the user's behalf.

Phantom will calculate and apply Priority Fees to all dApp-generated transactions, provided:

• The transaction(s) do not already have signature(s) present

• The transaction(s) do not have existing priority fee instructions (`computeUnitBudget` or `computeUnitLimit`)

• After enhancing transaction(s) with Priority Fees, the size of each transaction will still be less than the byte size limit (1232 bytes)

If all of the above conditions are met, Phantom will automatically calculate and apply priority fees at the time of signing. This pattern applies to all Phantom provider methods (`signAndSendTransaction`, `signTransaction` , `signAllTransactions`) across all environments (extension, mobile in-app browser, deeplinks, mobile wallet adapter).

### Further Reading

{% embed url="https://docs.solana.com/proposals/fee_transaction_priority" %}

{% embed url="https://solanacookbook.com/references/basic-transactions.html#how-to-change-compute-budget-fee-priority-for-a-transaction" %}

{% embed url="https://docs.solana.com/developing/programming-model/runtime#prioritization-fees" %}

{% embed url="https://docs.solana.com/transaction_fees#prioritization-fee" %}
