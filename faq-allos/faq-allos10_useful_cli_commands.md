# Useful Verus CLI commands.
Note: Read it completely before use.

##### General remarks on CLI wallet:
On Windows command line enter the commands as shown without the surrounding quotation marks
In Linux shell preceed the commands without surrounding quotation marks with ./
In MacOS shell preceed the commands without surrounding quotation marks with ./
for example the windows version verus listtransactions transforms in Linux or MacOS to ./verus listtransactions.
#####General remarks on Windows command line formatting:
The CLI help shows the command format for Linux and MacOS.
For windows substitute the shown '-character with the "-character.
For windows substitute the shown "-character with the \"-characters.
## Handy verus-cli commands:
Importing a VRSC private key into your wallet:
`verus importprivkey "PRIVATE_KEY"`

Getting your current VRSC balance:
`verus getbalance`

or for somewhat more information:
`verus z_gettotalbalance`

Sending VRSC coins from your verus wallet to and another VRSC address (t-address in this case, seperate command for z-addresses I think):
`verus sendtoaddress "VRSC_address" AMOUNT "Some comments here"`

Listing the latest VRSC transactions:
`verus listtransactions`

Send VRSC from PUBLIC address:
`verus`

Shield reward coins from all public addresses:
`verus z_shieldcoinbase "*" "Z-ADDRESS"`

Transfer X VRSX from (private) z-address to (public) R-address:
`verus z_sendmany "Z-ADDRESS" '[{"amount":X, "address":"R-ADDRESS"}]'`

Disclaimer: Always read up before using a verus-cli command, more info on each command can be found using the following:
    'verus help'

Added by @Crupti and @Oliver Westbrook.
