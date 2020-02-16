# Guide to change Agama from Lite Mode to Native Mode.

Attention: Read completely before use. Agama wallet is no longer supported.

#### Important General Information

`Agama Wallet location` on different OS:
Linux GUI: `~/.Komodo/VRSC`
Mac OS: `/Users//Library/Application Support/Komodo/VRSC`
Windows 10: `%AppData%\Roaming\Komodo\VRSC\`

#### Necessary files:

Link 2: Download Verus Bootstrap

## Procedure:
1) Make sure you have your seed phrase and password you use to log into your Lite mode wallet available.
2) First of all make a notition of your address and balance of VRSC you have in your wallet, before closing Agama.
3) Make sure the latest version of Agama Wallet for Verus is installed.
	3a) Download the latest Agama Verus Wallet (Agama is now unsupported)
	3b) Verify the SHA256 checksum of your download, so you have an untampered installer.
	3c) Run the file you just downloaded to install it.
4) Installing the bootstrap:
  4a) Download the bootstrap from Link 2.
  4b) Verify the SHA256 checksum of your download, to verify you have an untampered Bootstrap.
  4c) Unpack (using 7-zip when in Windows) the downloaded archive to `Agama Wallet location`.
5) Getting Agama ready for Native mode:
	5a) Start Agama and select **Native mode** VRSC.
	5b) You will get a red warning message about Zcash params. Dismiss it.
	5c) From the dropdown box, select `Z.cash` and click `Download`.
	5d) As soon as the download is finished, Agama will continu and bring you into your wallet. It will automatically start to synchronize the blockchain. Since we already put the majority of the chain in place, this will take just a few minutes.
6) Importing your existing Address:
	6a) Click `Import Key`.
	6b) Enter your seed in the top field or your WIF in the bottom field.
  6c) Select `Trigger rescan` and click `Import`.
	6d) You get a `Wallet Notification: Address imported` window. Dismiss it.
	6e) Check if your Balance is correct.

If you followed these steps, you will have updated to the latest version of Agama for verus, installed the bootstrap and imported your existing address into Agama. You can now Stake your balance and use Private (sapling) addresses.

Attention: Agama wallet is deprecated. This guide is not linked from the main page. It needs updating/reworking to Verus Desktop

Created by Oink.vrsc@
