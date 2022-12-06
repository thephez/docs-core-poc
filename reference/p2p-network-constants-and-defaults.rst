Constants and Defaults
**********************

The following constants and defaults are taken from Dash Core’s
`chainparams.cpp <https://github.com/dashpay/dash/blob/master/src/chainparams.cpp>`__
source code file.

======= ============================ =========== ==========
Network Default Port                 Magic Value <>
======= ============================ =========== ==========
Mainnet 9999                         0xBD6B0CBF  0xBF0C6BBD
Testnet 19999                        0xFFCAE2CE  0xCEE2CAFF
Regtest 19899                        0xDCB7C1FC  0xFCC1B7DC
Devnet  User-defined (default 19799) 0xCEFFCAE2  0xE2CAFFCE
======= ============================ =========== ==========

Note: the testnet start string and nBits above are for testnet3.

Command line parameters can change what port a <> listens on (see
``-help``). Start strings are hardcoded constants that appear at the
start of all messages sent on the Dash <>; they may also appear in data
files such as Dash Core’s block database. The Magic Value and <>
displayed above are in big-endian order; they’re sent over the network
in little-endian order. The <> is simply the endian reversed Magic
Value.

Dash Core’s
`chainparams.cpp <https://github.com/dashpay/dash/blob/master/src/chainparams.cpp>`__
also includes other constants useful to programs, such as the hash of
the <> blocks for the different networks.
