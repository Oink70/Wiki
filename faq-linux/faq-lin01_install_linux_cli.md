# How do I install the Linux CLI (command line interface) on a brand new (hosted) system?

Notice: Read it completely before use.

#### Important General Information

`wallet.dat` location on Linux: `~/.Komodo/VRSC`

#### Necessary files:

Link 1: [Download latest Wallet](https://veruscoin.io/wallet.html)
Link 2: [Download Verus Bootstrap](https://bootstrap.veruscoin.io/)

## Procedure:

1. First make sure your system is up to date:  
  `sudo apt-get update && apt-get upgrade -y`  

2. I suggest not to use the root account. I you have not yet done, set up a new user
  `sudo adduser newusername`
  `sudo usermod -aG sudo newusername`
3. Switch to new username
  `su -  newusername`
4. Test the your new user actually has root (SuDo) access, e.g.:
  `sudo ls -la /root`
  You should get some lines like this:
  `drwx------  6 root root 4096 Jul  3 15:56`
5. Download & install the wallet binaries:
  `wget https://github.com/VerusCoin/VerusCoin/releases/download/v0.6.4-1/Verus-CLI-Linux-v0.6.4-1-amd64.gz`
 The downloaded archive contains another archive and a signature text file, enabling the archive within to be verified (You'll need a running wallet to do that)
 Also: Verify the URL to the latest version from the [Download latest Wallet](https://veruscoin.io/wallet.html) above.
  `tar -xvf verus-cli-linux-v0.6.4.1.gz`
 Now extract the wallet archive:
  `tar -xvf verus-cli-linux-v0.6.4.1.tar.gz`
 Change directory to verus-cli
  `cd verus-cli`
 Fetch parameters, takes time, more on slow Internet connection
  `./fetch-params`
 Install libraries for Verus
  `sudo apt-get install libcurl3 g++-multilib -y`
 Install Tmux a terminal multiplexer with which you can run threads in the background see https://en.wikipedia.org/wiki/Tmux
  `sudo apt-get install tmux -y`
 Start tmux:
  `tmux`
 Launch Verus Daemon with or without number of threads
 (usually number of threads equals number of cores or double of that if the processor support hyper threading well)
  `./verusd -gen -genproclimit`
  `./verusd -gen -genproclimit=24`
 Once mining is operational – again this may take some time –
 you’ll see: 256 mega hashes complete - working
 then detach tmux
`[ctrl]&b d`

Disable login with Root User
(make sure your newly created user login works and has sudo rights)
  `sudo nano /etc/ssh/sshd_config`
Find: PermitRootLogin yes
And set to
PermitRootLogin no
Apply no settings:
  `sudo systemctl restart sshd`

(submitted by @karero, corrected by @Glennp, edited by Oink.vrsc@)

Note: last revision date 2020-02-25.
