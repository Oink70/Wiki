# Question: How to consolidate multiple `wallet.dat` files in one?

Attention: Read it completely before using.

### Verus `Wallet.dat`, Chaindata & `VRSC.conf` standard locations
Linux:		`~/.Komodo/VRSC`
Mac OS: 	`~/Library/Application Support/Komodo/VRSC`
Windows 10: 	`%AppData%\Roaming\Komodo\VRSC\`

## Procedure

1. Edit the `VRSC.conf` file on each instance you have, adding the following line: `exportdir=/LOCAL_PATH/` (ie /home/user/)
2. Restart your wallet
3. Issue the following command: `z_exportwallet FILENAME` (ie z_exportwallet export_instance01, the filename cannot have a `.` in it.)
4. Copy the generated file to the machine who host your main wallet
5. Issue the following command (on the "main" verus-cli): `z_importwallet /LOCAL_PATH/FILENAME` (ie /home/user/export_instance01)

note: These commands can be given in
* CLI wallet: `./verus z_exportwallet FILENAME`& `./verus z_importwallet /LOCAL_PATH/FILENAME`
* Verus Desktop in `settings`, `coin settings`: `run z_exportwallet FILENAME`& `run z_importwallet /LOCAL_PATH/FILENAME`
* Verus Agama in `settings`, `<CLI>`: `z_exportwallet FILENAME`& `z_importwallet /LOCAL_PATH/FILENAME`

(submitted by @TexWiller)

note: last revision date 2020-02-24.
