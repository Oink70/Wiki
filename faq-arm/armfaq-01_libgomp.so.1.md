# error while loading shared libraries: libgomp.so.1: No such file or directory

When running `./verusd` on an ARM64 ubuntu 18, not all dependencies may be installed on default, resulting in the errormessage `error while loading shared libraries: libgomp.so.1: No such file or directory`.

To solve this you need to install the libgomp library:
`sudo apt-get install libgomp1`

Solution supplied by: _PureHate.vrsc@

Note: creation date 2020-02-12.
