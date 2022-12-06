All messages in the network protocol use the same container format,
which provides a required multi-field <> and an optional payload. The
message header format is:

+------------+--------------------+---------------+------------------+
| Bytes      | Name               | Data Type     | Description      |
+============+====================+===============+==================+
| 4          | start string       | char[4]       | Magic bytes      |
|            |                    |               | indicating the   |
|            |                    |               | originating      |
|            |                    |               | network; used to |
|            |                    |               | seek to next     |
|            |                    |               | message when     |
|            |                    |               | stream state is  |
|            |                    |               | unknown.         |
+------------+--------------------+---------------+------------------+
| 12         | command name       | char[12]      | ASCII string     |
|            |                    |               | which identifies |
|            |                    |               | what message     |
|            |                    |               | type is          |
|            |                    |               | contained in the |
|            |                    |               | payload.         |
|            |                    |               | Followed by      |
|            |                    |               | nulls (0x00) to  |
|            |                    |               | pad out byte     |
|            |                    |               | count; for       |
|            |                    |               | example:         |
|            |                    |               | ``vers           |
|            |                    |               | ion\0\0\0\0\0``. |
+------------+--------------------+---------------+------------------+
| 4          | payload size       | uint32_t      | Number of bytes  |
|            |                    |               | in payload. The  |
|            |                    |               | current maximum  |
|            |                    |               | number of bytes  |
|            |                    |               | (```M            |
|            |                    |               | AX_SIZE`` <https |
|            |                    |               | ://github.com/da |
|            |                    |               | shpay/dash/blob/ |
|            |                    |               | v0.15.x/src/seri |
|            |                    |               | alize.h#L29>`__) |
|            |                    |               | allowed in the   |
|            |                    |               | payload by Dash  |
|            |                    |               | Core is 32       |
|            |                    |               | MiB—messages     |
|            |                    |               | with a payload   |
|            |                    |               | size larger than |
|            |                    |               | this will be     |
|            |                    |               | dropped or       |
|            |                    |               | rejected.        |
+------------+--------------------+---------------+------------------+
| 4          | checksum           | char[4]       | *Added in        |
|            |                    |               | protocol version |
|            |                    |               | 209.* First 4    |
|            |                    |               | bytes of         |
|            |                    |               | SHA256(          |
|            |                    |               | SHA256(payload)) |
|            |                    |               | in internal byte |
|            |                    |               | order. If        |
|            |                    |               | payload is       |
|            |                    |               | empty, as in     |
|            |                    |               | ``verack`` and   |
|            |                    |               | ```getaddr``     |
|            |                    |               | messages <co     |
|            |                    |               | re-ref-p2p-netwo |
|            |                    |               | rk-control-messa |
|            |                    |               | ges#getaddr>`__, |
|            |                    |               | the checksum is  |
|            |                    |               | always           |
|            |                    |               | 0x5df6e0e2       |
|            |                    |               | (SH              |
|            |                    |               | A256(SHA256())). |
+------------+--------------------+---------------+------------------+

Example
=======

The following example is an annotated hex dump of a <> message header
from a ```verack``
message <core-ref-p2p-network-control-messages#verack>`__ which has no
payload.

.. code:: text

   bf0c6bbd ................... Start string: Mainnet
   76657261636b000000000000 ... Command name: verack + null padding
   00000000 ................... Byte count: 0
   5df6e0e2 ................... Checksum: SHA256(SHA256(<empty>))
