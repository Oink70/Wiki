# Guide to change Verus-Desktop from Lite Mode to Native Mode.

Attention: Read completely before use. Agama wallet is no longer supported.

#### Important General Information

`VRSC wallet & data location` on different OS:
Linux GUI: `~/.Komodo/VRSC`
Mac OS: `/Users//Library/Application Support/Komodo/VRSC`
Windows 10: `%AppData%\Roaming\Komodo\VRSC\`

#### Necessary files & links:

Link 1: [Download Verus Bootstrap](https://bootstrap.veruscoin.io)
Link 2: [Import Lite wallet address in Verus Desktop native](https://wiki.veruscoin.io/#!how-to/how-to_convert-seed-to-wif.md)

## Procedure:
1. Make sure you have your seed phrase and password you use to log into your Lite mode wallet available.
2. First of all make a notition of your address and balance of VRSC you have in your wallet, before closing Agama.
3. Make sure the latest version of Agama Wallet for Verus is installed.
 1. Download the latest Agama Verus Wallet (Agama is now unsupported)
 2. Verify the SHA256 checksum of your download, so you have an untampered installer.
 3. Run the file you just downloaded to install it.
4. Installing the bootstrap:
  1. Download the bootstrap from [Link 1](https://bootstrap.veruscoin.io).
  2. Verify the SHA256 checksum of your download, to verify you have an untampered Bootstrap.
  3. Unpack the downloaded archive to `VRSC wallet & data location`.
5. Getting Verus-Desktop ready for Native mode:
	1. Start Agama and select **Native mode** VRSC.
	2. You may get a red warning message about Zcash params.
	3. Verus Desktop will detect if you have the necessary ZCash parameter files and download them if needed.
	4. As soon as the download is finished, Verus-Desktop will continu and bring you into your wallet. It will automatically start to synchronize the blockchain. Since we already put the majority of the chain in place, this will take just a few minutes.
6. Importing your existing Address:
	* This procedure is described in detail in: [Import Lite wallet address in Verus Desktop native](https://wiki.veruscoin.io/#!how-to/how-to_convert-seed-to-wif.md).

If you followed these steps, installed the bootstrap, switched from Lite to Native mode and imported your existing address into Verus-Desktop. You can now stake your balance and use Private (sapling) addresses.

Created by Oink.vrsc@

Note: last revision date 2020-02-26.
