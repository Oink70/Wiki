# Question: How to consolidate multiple `wallet.dat` files in one?

Answer:
1) Edit the VRSC.conf file on each instance you have, adding the following line: exportdir=/LOCAL_PATH/ (ie /home/user/)
2) Restart your wallet
3) Issue the following command: `z_exportwallet FILENAME` (ie z_exportwallet export_instance01, the filename cannot have a `.` in it.)
4) Copy the generated file to the machine who host your main wallet
5) Issue the following command (on the "main" verus-cli): `z_importwallet /LOCAL_PATH/FILENAME` (ie /home/user/export_instance01)

Note: for the location of VRSC.conf in different OS's, please check [Verus Wallet & VRSC.conf standard locations](#24_wallet.dat_and_vrsc.conf_location.md)
(submitted by @TexWiller)
