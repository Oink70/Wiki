# How to import your import your Lite wallet address into your native Verus Desktop?

Attention: Read it completely before using.

## Prerequisites

In order to convert your seedphrase into a Private key (WIF), you need to have a running native wallet first, that is fully synchronized. The earliest wallet that supports these functions is **Verus Desktop v0.6.4-beta-1**.

If needed, use this guide to quickly synchronize your wallet: https://wiki.veruscoin.io/#!how-to/how-to_bootstrap.md

## Procedure
If you have a seed, you can retrieve your Private key (WIF) by having the Verus Desktop wallet convert it for you.
To convert your *seed phrase* in Verus Desktop, go to `settings` --> `Coin Settings` and enter the following command:
```
run convertpassphrase "seedphrase"
```
You will receive a response similar to this:
```
{
"walletpassphrase": "seedphrase",
"address": "RYX6RYU3AAvwVCNyNM4cVyGUhSMUPvKs3r",
"pubkey": "02ffc2f4b071afdec631e3fb7d435a0047be14a81ea1a269e4206b0068c0c1fa6f",
"privkey": "d899ed88e9ee2e90c2cf51cb47e7b4495ec1e1cb10763bb1c111b0bde48bf86c",
"wif": "UwGb5KvGPfMUr1tu74Desjh87ZeJM4wq5goLyThcogeLifc5aJqT"
}
```
Copy that information and store it somewhere **SAFE**. With this information anyone having access to it will have full control over that address.

The **wif** that is shown is the data you want to import in the next step.

To import your address, go to `settings` --> `Coin Settings` and enter the following command:
```
run importprivkey "<wif>" "" true
```
Replace `<wif>` with the actual **wif** you got from the `convertpassphrase` command earlier.

The GUI wallet will not show any progress on the import and may give messages that the RPC daemon is not reacting. It will take quite some time for the process to finish in the background, especially if the address has many transaction on it.

Information compiled by Oink.vrsc@.

Note: revision date 2020-02-16.
