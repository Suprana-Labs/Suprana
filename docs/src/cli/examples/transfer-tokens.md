---
title: Send and Receive Tokens with the Suprana CLI
pagination_label: "Suprana CLI: Send and Receive Tokens"
sidebar_label: Send and Receive Tokens
---

This page describes how to receive and send SOL tokens using the command line
tools with a command line wallet such as a [paper wallet](../wallets/paper.md),
a [file system wallet](../wallets/file-system.md), or a
[hardware wallet](../wallets/hardware/index.md). Before you begin, make sure
you have created a wallet and have access to its address (pubkey) and the
signing keypair. Check out our
[conventions for entering keypairs for different wallet types](../intro.md#keypair-conventions).

## Testing your Wallet

Before sharing your public key with others, you may want to first ensure the
key is valid and that you indeed hold the corresponding private key.

In this example, we will create a second wallet in addition to your first wallet,
and then transfer some tokens to it. This will confirm that you can send and
receive tokens on your wallet type of choice.

This test example uses our Developer Testnet, called devnet. Tokens issued
on devnet have **no** value, so don't worry if you lose them.

#### Airdrop some tokens to get started

First, _airdrop_ yourself some play tokens on the devnet.

```bash
suprana airdrop 1 <RECIPIENT_ACCOUNT_ADDRESS> --url https://api.devnet.suprana.net
```

where you replace the text `<RECIPIENT_ACCOUNT_ADDRESS>` with your base58-encoded
public key/wallet address.

A response with the signature of the transaction will be returned. If the balance
of the address does not change by the expected amount, run the following command
for more information on what potentially went wrong:

```bash
suprana confirm -v <TRANSACTION_SIGNATURE>
```

#### Check your balance

Confirm the airdrop was successful by checking the account's balance.
It should output `1 SOL`:

```bash
suprana balance <ACCOUNT_ADDRESS> --url https://api.devnet.suprana.net
```

#### Create a second wallet address

We will need a new address to receive our tokens. Create a second
keypair and record its pubkey:

```bash
suprana-keygen new --no-passphrase --no-outfile
```

The output will contain the address after the text `pubkey:`. Copy the
address. We will use it in the next step.

```text
pubkey: GKvqsuNcnwWqPzzuhLmGi4rzzh55FhJtGizkhHaEJqiV
```

You can also create a second (or more) wallet of any type:
[paper](../wallets/paper.md#creating-multiple-paper-wallet-addresses),
[file system](../wallets/file-system.md#creating-multiple-file-system-wallet-addresses),
or [hardware](../wallets/hardware/index.md#multiple-addresses-on-a-single-hardware-wallet).

#### Transfer tokens from your first wallet to the second address

Next, prove that you own the airdropped tokens by transferring them.
The Suprana cluster will only accept the transfer if you sign the transaction
with the private keypair corresponding to the sender's public key in the
transaction.

```bash
suprana transfer --from <KEYPAIR> <RECIPIENT_ACCOUNT_ADDRESS> 0.5 --allow-unfunded-recipient --url https://api.devnet.suprana.net --fee-payer <KEYPAIR>
```

where you replace `<KEYPAIR>` with the path to a keypair in your first wallet,
and replace `<RECIPIENT_ACCOUNT_ADDRESS>` with the address of your second
wallet.

Confirm the updated balances with `suprana balance`:

```bash
suprana balance <ACCOUNT_ADDRESS> --url http://api.devnet.suprana.net
```

where `<ACCOUNT_ADDRESS>` is either the public key from your keypair or the
recipient's public key.

#### Full example of test transfer

```bash
$ suprana-keygen new --outfile my_suprana_wallet.json   # Creating my first wallet, a file system wallet
Generating a new keypair
For added security, enter a passphrase (empty for no passphrase):
Wrote new keypair to my_suprana_wallet.json
==========================================================================
pubkey: DYw8jCTfwHNRJhhmFcbXvVDTqWMEVFBX6ZKUmG5CNSKK                          # Here is the address of the first wallet
==========================================================================
Save this seed phrase to recover your new keypair:
width enhance concert vacant ketchup eternal spy craft spy guard tag punch    # If this was a real wallet, never share these words on the internet like this!
==========================================================================

$ suprana airdrop 1 DYw8jCTfwHNRJhhmFcbXvVDTqWMEVFBX6ZKUmG5CNSKK --url https://api.devnet.suprana.net  # Airdropping 1 SOL to my wallet's address/pubkey
Requesting airdrop of 1 SOL from 35.233.193.70:9900
1 SOL

$ suprana balance DYw8jCTfwHNRJhhmFcbXvVDTqWMEVFBX6ZKUmG5CNSKK --url https://api.devnet.suprana.net # Check the address's balance
1 SOL

$ suprana-keygen new --no-outfile  # Creating a second wallet, a paper wallet
Generating a new keypair
For added security, enter a passphrase (empty for no passphrase):
====================================================================
pubkey: 7S3P4HxJpyyigGzodYwHtCxZyUQe9JiBMHyRWXArAaKv                   # Here is the address of the second, paper, wallet.
====================================================================
Save this seed phrase to recover your new keypair:
clump panic cousin hurt coast charge engage fall eager urge win love   # If this was a real wallet, never share these words on the internet like this!
====================================================================

$ suprana transfer --from my_suprana_wallet.json 7S3P4HxJpyyigGzodYwHtCxZyUQe9JiBMHyRWXArAaKv 0.5 --allow-unfunded-recipient --url https://api.devnet.suprana.net --fee-payer my_suprana_wallet.json  # Transferring tokens to the public address of the paper wallet
3gmXvykAd1nCQQ7MjosaHLf69Xyaqyq1qw2eu1mgPyYXd5G4v1rihhg1CiRw35b9fHzcftGKKEu4mbUeXY2pEX2z  # This is the transaction signature

$ suprana balance DYw8jCTfwHNRJhhmFcbXvVDTqWMEVFBX6ZKUmG5CNSKK --url https://api.devnet.suprana.net
0.499995 SOL  # The sending account has slightly less than 0.5 SOL remaining due to the 0.000005 SOL transaction fee payment

$ suprana balance 7S3P4HxJpyyigGzodYwHtCxZyUQe9JiBMHyRWXArAaKv --url https://api.devnet.suprana.net
0.5 SOL  # The second wallet has now received the 0.5 SOL transfer from the first wallet

```

## Receive Tokens

To receive tokens, you will need an address for others to send tokens to. In
Suprana, the wallet address is the public key of a keypair. There are a variety
of techniques for generating keypairs. The method you choose will depend on how
you choose to store keypairs. Keypairs are stored in wallets. Before receiving
tokens, you will need to [create a wallet](../wallets/index.md).
Once completed, you should have a public key
for each keypair you generated. The public key is a long string of base58
characters. Its length varies from 32 to 44 characters.

## Send Tokens

If you already hold SOL and want to send tokens to someone, you will need
a path to your keypair, their base58-encoded public key, and a number of
tokens to transfer. Once you have that collected, you can transfer tokens
with the `suprana transfer` command:

```bash
suprana transfer --from <KEYPAIR> <RECIPIENT_ACCOUNT_ADDRESS> <AMOUNT> --fee-payer <KEYPAIR>
```

Confirm the updated balances with `suprana balance`:

```bash
suprana balance <ACCOUNT_ADDRESS>
```
