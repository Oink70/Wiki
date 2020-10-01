# How-To: Backup my wallet?

## Important General Information

## Optional:
Watch this video with an explanation how to accomplish the steps below: [Backup your wallet](https://youtu.be/ygPsdK8Trck)


### Verus `Wallet.dat`, Chaindata & `VRSC.conf` standard locations
 * Linux:		`~/.Komodo/VRSC`
 * Mac OS: 	`~/Library/Application Support/Komodo/VRSC`
 * Windows 10: 	`%AppData%\Roaming\Komodo\VRSC\`
 * OS independent through Verus Desktop: Click `help`, `Show Verus data folder (default)`

Note: For Komodo the base directory is `komodo`.
For Komodo asset chains it is a folder/directory in the `komodo` base directory (eg `komodo/PIRATE`) with the **official** coin designation.
For Zcash the base directory is `zcash` instead of komodo.

`verus command "<userinput>"` needs to be entered literally, with the text between and including `<` and `>` replaced by your specific user data. So if the text directs you to use for example `"<Public Address>"`, you replace that (including the `<` and `>`) with the address,
so it looks similar to this: `"RYX6RYU3AAvwVCNyNM4cVyGUhSMUPvKs3r"`.

## Backing up your `wallet.dat`

Note: The filename you replace`<DestinationFileName>` with, can only contain letters and figures, no other characters, so it **cannot** have an file-extension

Attention: On VRSC the `exportdir=<dir>` is by default enabled to the standard chain folder. Other chains may need this entry in the coins configuration file to specify the export directory, before this can be used.

#### Verus Desktop:
   Go to `Settings`, `Coin Settings` en click in the textbox shown there.
   Enter `run backupwallet "<DestinationFileName>"` and press enter to execute the command.
#### linux/MacOS CLI:
   run `./verus backupwallet "<DestinationFileName>"`
#### windows CLI:
   run `verus backupwallet "<DestinationFileName>"`

   Attention: Pay attention to the feedback this command gives you: it will mention the location the backup file is saved.

   The backup wallet should be a file called `<DestinationFileName>`, standard in the same directory as your `wallet.dat`. Keep this file secure, it has your plaintext private keys. Verify that the file is there and is the same size as your `wallet.dat`.

## Exporting your wallet

Note: The filename you replace`<mywalletexport>` with, can only contain letters and figures, no other characters, so it **cannot** have an file-extension

Attention: On VRSC the `exportdir=<dir>` is by default enabled to the standard chain folder. Other chains may need this entry in the coins configuration file to specify the export directory, before this can be used.

#### Verus Desktop:
   Go to `Settings`, `Coin Settings` and click in the textbox shown there.
   Enter `run z_exportwallet "<mywalletexport>"` and press enter to execute the command.
#### linux/MacOS CLI:
   run `./verus z_exportwallet "<mywalletexport>"`
#### windows CLI:
   run `verus z_exportwallet "<mywalletexport>"`

Attention: Pay attention to the feedback this command gives you: it will mention the location the backup file is saved.

The exported wallet should be a file called `<mywalletexport>`, standard in the same directory as your `wallet.dat`. Keep this file secure, it has your plaintext private keys. Verify that the file is there and isn't empty.

Information compiled by Oink.vrsc@.

Note: revision date 2020-10-01.
