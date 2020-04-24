# My wallet is stuck on block number XXXX. It does not synchronize properly anymore.

note: Read it completely before use.

#### Wallet & blockchain location:
Linux GUI:    `~/.Komodo/VRSC`
Mac OS:       `/Users//Library/Application Support/Komodo/VRSC`
Windows 10:   `%AppData%\Komodo\VRSC\`

#### Usefull links:
Link 1: [Download latest Wallet](https://veruscoin.io/wallet.html)
Link 2: [Show current blockheight](https://explorer.veruscoin.io/api/getblockcount)

## Procedure:
In case your wallet is not synchronized with the blockchain and restarting Agama doesn't connect to any peers:

Compare your blockheight with the one Link 2 above is showing to make sure you are
not synchronized anymore. If the blockheight of the link above is significantly higher (more than 10) than
the blockheight your wallet is showing, follow the rest of the procedure.
If the numbers are equal or close, your wallet is synchronized and the procedure below will not solve any problems.

Close your wallet.
Go to the appropriate location for your OS as mentioned above.

Add a similar list to the bottom of your `VRSC.conf`, just below `rpcallowip=127.0.0.1`:
  addnode=95.216.252.182
  addnode=104.18.42.17
Save and exit the file.
An up-to-date list of working nodes can be found in Verus Discord in the #tipbot channel, by messaging `!vrsctip peers` in that channel.

After you added those 2 nodes, remove every **file** that is directly in VRSC folder, none from the subfolders
(or at least move to a different location) except `wallet.dat` and `vrsc.conf`.
Make sure you don't remove any folders, or you'll have to use the bootstrap.

Then start your wallet as you're used to.

Submitted by Oink.vrsc@ & Thoskk.vrsc@

Note: last revision date 2020-04-24.
