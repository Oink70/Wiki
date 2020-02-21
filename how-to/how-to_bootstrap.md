# Guide to install Bootstrap for your Verus Wallet.

Attention: Read it completely before using.

### Important General Information

`VRSC Wallet location` on different OS:
Linux GUI: `~/.komodo/VRSC`
Mac OS: `/Users//Library/Application Support/komodo/VRSC`
Windows 10: `%AppData%\Komodo\VRSC\`

Tip: The easiest way is to copy the location above and past it into your address bar of your file browser. Your operation system will accept the input, interpret where that location is and bring you there.

#### NEW:

##### Automated bootstrap scripts:

[Windows 10](https://github.com/Oink70/VerusExtras/releases/download/v1.0.3/VRSC-bootstrap-win.bat) (Tested on build 18362.418)
[Linux](https://github.com/Oink70/VerusExtras/releases/download/v1.0.3/VRSC-bootstrap-linux.sh) (Tested on Ubuntu 18.04)
[MacOS](https://github.com/Oink70/VerusExtras/releases/download/v1.0.3/VRSC-bootstrap-mac.command) (Tested on Mac OS 10.14 Mojave)

Tip: The bootstraps above do not delete any information, don't update your wallet to the latest version and don't make a backup of your wallet. If you are instructed to use the manual procedure, don't use these scripts.

## Necessary files:

Link 1: [Download latest Wallet](https://veruscoin.io/wallet.html)
Link 2: [Download Verus Bootstrap](https://bootstrap.veruscoin.io/)

## Manual procedure:

If you do not feel confident in executing the instructions listed below, consider using the automated bootstrap script
available for your Operating system. In some cases you will be asked by community members to execute the manual procedure,
since the script does not delete any files, it only overwrites the chain data with the most recent version from the
bootstrap archive.

1. Make sure your wallet is not active.
2. If you already had you wallet running, backup essential files:
	a. Go to `VRSC Wallet location`
	b. copy `wallet.dat` to a *SAFE* location
	c. copy `VRSC.conf` to a *SAFE* location
	d. Verify that both files are copied to your safe location
3. Make sure the latest version of your Wallet for Verus is installed
	a. Download the latest Verus Wallet from link 1, supplied above.
	b. Verify the SHA256 checksum & signature of your download, to verify you have an untampered installer.
	c. extract the file you just downloaded to a suitable location.
4. Installing the bootstrap:
  a. Download the bootstrap from Link 2. (For Windows a zip archive is available to accomodate native extraction. Linux and MacOS users can use the tar.gz archive)
  b. (Optional, but recommended) Verify the md5, sha256 or sha512 checksum and the signature of your download, to verify that you downloaded an untampered Bootstrap archive.
  c. Remove all files and folders from `VRSC Wallet Location` except `wallet.dat` and `VRSC.conf`.

Attention: If you have downloaded, copied or moved the bootstrap archive to the `VRSC Wallet location`, do not delete that archive when removing files.

  d. Unpack the downloaded archive to `VRSC Wallet location`.
5. If you had a VRSC wallet running before, restore essential files:
	a. Go to `VRSC Wallet location`
	b. Verify that your `wallet.dat` is bigger than the one in this folder (if any is present)
	c. copy `wallet.dat` from your *SAFE* location
6. Start your wallet

If you followed these steps, you will have installed/updated the latest version of a wallet for verus, made a backup of your wallet and installed the bootstrap. If desired you can remove the downloaded bootstrap archive to free up space on your hard drive.

Information compiled by Oink.vrsc@.

Note: revision date 2020-02-20.
