The hashes used in P2PKH and <> are commonly encoded as Dash <>. This is
the procedure to encode those hashes and decode the addresses.

Conversion Process
==================

First, get your hash. For P2PKH, you RIPEMD-160(SHA256()) hash a ECDSA
<> derived from your 256-bit ECDSA <> (random data). For P2SH, you
RIPEMD-160(SHA256()) hash a <> serialized in the format used in <>
(described in a `following
sub-section <core-ref-transactions-raw-transaction-format>`__). Taking
the resulting hash:

1. Add an <> version byte in front of the hash. The version bytes
   commonly used by Dash are:

   -  0x4c for <> on the main Dash network (<>)

   -  0x8c for <> on the Dash testing network (<>)

   -  0x10 for <> on <>

   -  0x13 for <> on <>

2. Create a copy of the version and hash; then hash that twice with
   SHA256: ``SHA256(SHA256(version . hash))``

3. Extract the first four bytes from the double-hashed copy. These are
   used as a checksum to ensure the base hash gets transmitted
   correctly.

4. Append the checksum to the version and hash, and encode it as a <>
   string: ``BASE58(version . hash . checksum)``

Example Code
============

Dash’s base58 encoding, called <> may not match other implementations.
Tier Nolan provided the following example encoding algorithm to the
Bitcoin Wiki `Base58Check
encoding <https://en.bitcoin.it/wiki/Base58Check_encoding>`__ page under
the `Creative Commons Attribution 3.0
license <https://creativecommons.org/licenses/by/3.0/>`__:

.. code:: c

   code_string = "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"
   x = convert_bytes_to_big_integer(hash_result)

   output_string = ""

   while(x > 0)
      {
          (x, remainder) = divide(x, 58)
          output_string.append(code_string[remainder])
      }

   repeat(number_of_leading_zero_bytes_in_hash)
      {
      output_string.append(code_string[0]);
      }

   output_string.reverse();

Dash’s own code can be traced using the `base58 header
file <https://github.com/dashpay/dash/blob/master/src/base58.h>`__.

To convert addresses back into hashes, reverse the base58 encoding,
extract the checksum, repeat the steps to create the checksum and
compare it against the extracted checksum, and then remove the version
byte.
