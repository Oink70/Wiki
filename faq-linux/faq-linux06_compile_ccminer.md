# Compile Monkins Verus enhanced CCMiner for various hardware
Read it completely before using.

## Important General Information
'Verus Wallet location` on Linux: `~/.Komodo/VRSC`
There are 3 active branches in ccminer github repo:
  ARM             (for 64bit ARM chips with AES intrinsic)
  Verus2.1        (standard x86-64 pc's)
  Verus2.1gpu     (GPUs)

Note: Replace in step 2 ARM with the branchname above you want to use.

## Procedure:
1) sudo apt-get install libcurl4-openssl-dev libssl-dev libjansson-dev automake autotools-dev build-essential
2) git clone --single-branch -b ARM https://github.com/monkins1010/ccminer.git
3) cd ccminer
4) chmod +x build.sh
5) chmod +x configure.sh
6) chmod +x autogen.sh
7) ./build.sh
And finally starting the miner (Change pool, address & workername to your own liking):
8) ./ccminer -a verus -o stratum+tcp://pool.veruscoin.io:9999 -u RVjvbZuqSGLGDm1B9BFkbHWySPKEx4tfjQ.donator -p x

Info from @Chris - Monkins1010 LOUD Mining.

Note: last revision date 2020-03-12.
