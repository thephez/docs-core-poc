Control Messages
****************

The following <> messages all help control the connection between two <>
or allow them to advise each other about the rest of the network.

.. figure:: https://dash-docs.github.io/img/dev/en-p2p-control-messages.svg
   :alt: Overview Of P2P Protocol Control And Advisory Messages

   Overview Of P2P Protocol Control And Advisory Messages

Note that almost none of the control messages are authenticated in any
way, meaning they can contain incorrect or intentionally harmful
information.

addr
====

The ``addr`` (IP address) message relays connection information for
peers on the network. Each peer which wants to accept incoming
connections creates an ```addr``
message <core-ref-p2p-network-control-messages#addr>`__ providing its
connection information and then sends that message to its peers
unsolicited. Some of its peers send that information to their peers
(also unsolicited), some of which further distribute it, allowing
decentralized peer discovery for any program already on the network.

An ```addr`` message <core-ref-p2p-network-control-messages#addr>`__ may
also be sent in response to a ```getaddr``
message <core-ref-p2p-network-control-messages#getaddr>`__.

+------------+------------------+--------------------+----------------+
| Bytes      | Name             | Data Type          | Description    |
+============+==================+====================+================+
| *Varies*   | IP address count | compactSize uint   | The number of  |
|            |                  |                    | IP address     |
|            |                  |                    | entries up to  |
|            |                  |                    | a maximum of   |
|            |                  |                    | 1,000.         |
+------------+------------------+--------------------+----------------+
| *Varies*   | IP addresses     | network IP address | IP address     |
|            |                  |                    | entries. See   |
|            |                  |                    | the table      |
|            |                  |                    | below for the  |
|            |                  |                    | format of a    |
|            |                  |                    | Dash network   |
|            |                  |                    | IP address.    |
+------------+------------------+--------------------+----------------+

Each encapsulated network IP address currently uses the following
structure:

+----------+------------------+----------------+-----------------------+
| Bytes    | Name             | Data Type      | Description           |
+==========+==================+================+=======================+
| 4        | time             | uint32         | *Added in protocol    |
|          |                  |                | version 31402.* A     |
|          |                  |                | time in Unix epoch    |
|          |                  |                | time format. Nodes    |
|          |                  |                | advertising their own |
|          |                  |                | IP address set this   |
|          |                  |                | to the current time.  |
|          |                  |                | Nodes advertising IP  |
|          |                  |                | addresses they’ve     |
|          |                  |                | connected to set this |
|          |                  |                | to the last time they |
|          |                  |                | connected to that     |
|          |                  |                | node. Other nodes     |
|          |                  |                | just relaying the IP  |
|          |                  |                | address should not    |
|          |                  |                | change the time.      |
|          |                  |                | Nodes can use the     |
|          |                  |                | time field to avoid   |
|          |                  |                | relaying old          |
|          |                  |                | ```addr``             |
|          |                  |                | messages <core-       |
|          |                  |                | ref-p2p-network-contr |
|          |                  |                | ol-messages#addr>`__. |
|          |                  |                | Malicious nodes may   |
|          |                  |                | change times or even  |
|          |                  |                | set them in the       |
|          |                  |                | future.               |
+----------+------------------+----------------+-----------------------+
| 8        | services         | uint64_t       | The services the node |
|          |                  |                | advertised in its     |
|          |                  |                | ```version``          |
|          |                  |                | message <core-ref     |
|          |                  |                | -p2p-network-control- |
|          |                  |                | messages#version>`__. |
+----------+------------------+----------------+-----------------------+
| 16       | IP address       | char           | IPv6 address in **big |
|          |                  |                | endian byte order**.  |
|          |                  |                | IPv4 addresses can be |
|          |                  |                | provided as           |
|          |                  |                | `IPv4-mapped IPv6     |
|          |                  |                | addresses <h          |
|          |                  |                | ttp://en.wikipedia.or |
|          |                  |                | g/wiki/IPv6#IPv4-mapp |
|          |                  |                | ed_IPv6_addresses>`__ |
+----------+------------------+----------------+-----------------------+
| 2        | port             | uint16_t       | Port number in **big  |
|          |                  |                | endian byte order**.  |
|          |                  |                | Note that Dash Core   |
|          |                  |                | will only connect to  |
|          |                  |                | nodes with            |
|          |                  |                | non-standard port     |
|          |                  |                | numbers as a last     |
|          |                  |                | resort for finding    |
|          |                  |                | peers. This is to     |
|          |                  |                | prevent anyone from   |
|          |                  |                | trying to use the     |
|          |                  |                | network to disrupt    |
|          |                  |                | non-Dash services     |
|          |                  |                | that run on other     |
|          |                  |                | ports.                |
+----------+------------------+----------------+-----------------------+

The following annotated hexdump shows part of an ```addr``
message <core-ref-p2p-network-control-messages#addr>`__. (The <> has
been omitted and the actual IP address has been replaced with a
`RFC5737 <http://tools.ietf.org/html/rfc5737>`__ reserved IP address.)

.. code:: text

   fde803 ............................. Address count: 1000

   d91f4854 ........................... Epoch time: 1414012889
   0100000000000000 ................... Service bits: 01 (network node)
   00000000000000000000ffffc0000233 ... IP Address: ::ffff:192.0.2.51
   208d ............................... Port: 8333

   [...] .............................. (999 more addresses omitted)

addrv2
======

*Added in protocol version 70220 of Dash Core*

The ``addrv2`` message relays connection information for peers on the
network just like the ```addr`` message <#addr>`__, but is extended to
allow gossiping of longer node addresses as described in
`BIP155 <https://github.com/bitcoin/bips/blob/master/bip-0155.mediawiki>`__.

Each encapsulated network IP address currently uses the following
structure:

+----------+------------------+----------------+-----------------------+
| Bytes    | Name             | Data Type      | Description           |
+==========+==================+================+=======================+
| 4        | time             | uint32_t       | A time in Unix epoch  |
|          |                  |                | time format. Nodes    |
|          |                  |                | advertising their own |
|          |                  |                | IP address set this   |
|          |                  |                | to the current time.  |
|          |                  |                | Nodes advertising IP  |
|          |                  |                | addresses they’ve     |
|          |                  |                | connected to set this |
|          |                  |                | to the last time they |
|          |                  |                | connected to that     |
|          |                  |                | node. Other nodes     |
|          |                  |                | just relaying the IP  |
|          |                  |                | address should not    |
|          |                  |                | change the time.      |
+----------+------------------+----------------+-----------------------+
| Varies   | services         | compactSize    | The services the node |
|          |                  | uint           | advertised in its     |
|          |                  |                | ```version``          |
|          |                  |                | message <core-ref     |
|          |                  |                | -p2p-network-control- |
|          |                  |                | messages#version>`__. |
+----------+------------------+----------------+-----------------------+
| 1        | networkID        | uint8_t        | Network identifier.   |
|          |                  |                | An 8-bit value that   |
|          |                  |                | specifies which       |
|          |                  |                | network is addressed. |
|          |                  |                | Network ID types may  |
|          |                  |                | be found in           |
|          |                  |                | `BIP15                |
|          |                  |                | 5 <https://github.com |
|          |                  |                | /bitcoin/bips/blob/ma |
|          |                  |                | ster/bip-0155.mediawi |
|          |                  |                | ki#specification>`__. |
+----------+------------------+----------------+-----------------------+
| Varies   | addr             | Vector         | Network address. The  |
|          |                  |                | interpretation        |
|          |                  |                | depends on networkID. |
+----------+------------------+----------------+-----------------------+
| 2        | port             | uint16_t       | Port number in **big  |
|          |                  |                | endian byte order**.  |
|          |                  |                | Note that Dash Core   |
|          |                  |                | will only connect to  |
|          |                  |                | nodes with            |
|          |                  |                | non-standard port     |
|          |                  |                | numbers as a last     |
|          |                  |                | resort for finding    |
|          |                  |                | peers. This is to     |
|          |                  |                | prevent anyone from   |
|          |                  |                | trying to use the     |
|          |                  |                | network to disrupt    |
|          |                  |                | non-Dash services     |
|          |                  |                | that run on other     |
|          |                  |                | ports.                |
+----------+------------------+----------------+-----------------------+

The following annotated hexdump shows part of an ``addrv2`` message.
(The <> has been omitted.)

.. code:: text

   01 ................................. Address count: 1

   1a2a8961 ........................... Epoch time: 1636379162

   Services information
   | fd ............................... Number of service bytes (2)
   | 0504 ............................. Service bits: 10000000101 (network, bloom, network_limited)

   Peer address details
   | 01 ............................... Network ID (1 - IPv4)
   | 04 ............................... Address length
   | 2d20ed4c ......................... IP Address: 45.32.237.76
   | 4a37 ............................. Port: 18999

filteradd
=========

*Added in protocol version 70001 as described by BIP37.*

The ```filteradd``
message <core-ref-p2p-network-control-messages#filteradd>`__ tells the
receiving <> to add a single element to a previously-set <>, such as a
new <>. The element is sent directly to the receiving peer; the peer
then uses the parameters set in the ```filterload``
message <core-ref-p2p-network-control-messages#filterload>`__ to add the
element to the bloom filter.

Because the element is sent directly to the receiving peer, there is no
obfuscation of the element and none of the plausible-deniability privacy
provided by the bloom filter. Clients that want to maintain greater
privacy should recalculate the bloom filter themselves and send a new
```filterload``
message <core-ref-p2p-network-control-messages#filterload>`__ with the
recalculated bloom filter.

+-----------+-----------------+--------------------+-------------------+
| Bytes     | Name            | Data Type          | Description       |
+===========+=================+====================+===================+
| *Varies*  | element bytes   | compactSize uint   | The number of     |
|           |                 |                    | bytes in the      |
|           |                 |                    | following element |
|           |                 |                    | field.            |
+-----------+-----------------+--------------------+-------------------+
| *Varies*  | element         | uint8_t[]          | The element to    |
|           |                 |                    | add to the        |
|           |                 |                    | current filter.   |
|           |                 |                    | Maximum of 520    |
|           |                 |                    | bytes, which is   |
|           |                 |                    | the maximum size  |
|           |                 |                    | of an element     |
|           |                 |                    | which can be      |
|           |                 |                    | pushed onto the   |
|           |                 |                    | stack in a pubkey |
|           |                 |                    | or signature      |
|           |                 |                    | script. Elements  |
|           |                 |                    | must be sent in   |
|           |                 |                    | the byte order    |
|           |                 |                    | they would use    |
|           |                 |                    | when appearing in |
|           |                 |                    | a raw             |
|           |                 |                    | transaction; for  |
|           |                 |                    | example, hashes   |
|           |                 |                    | should be sent in |
|           |                 |                    | internal byte     |
|           |                 |                    | order.            |
+-----------+-----------------+--------------------+-------------------+

Note: a ```filteradd``
message <core-ref-p2p-network-control-messages#filteradd>`__ will not be
accepted unless a filter was previously set with the ```filterload``
message <core-ref-p2p-network-control-messages#filterload>`__.

The annotated hexdump below shows a ```filteradd``
message <core-ref-p2p-network-control-messages#filteradd>`__ adding a
<>. (The message header has been omitted.) This TXID appears in the same
block used for the example hexdump in the ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__; if that
```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ is re-sent
after sending this ```filteradd``
message <core-ref-p2p-network-control-messages#filteradd>`__, six hashes
are returned instead of four.

.. code:: text

   20 ................................. Element bytes: 32
   fdacf9b3eb077412e7a968d2e4f11b9a
   9dee312d666187ed77ee7d26af16cb0b ... Element (A TXID)

filterclear
===========

*Added in protocol version 70001 as described by BIP37.*

The ```filterclear``
message <core-ref-p2p-network-control-messages#filterclear>`__ tells the
receiving <> to remove a previously-set <>. This also undoes the effect
of setting the relay field in the ```version``
message <core-ref-p2p-network-control-messages#version>`__ to 0,
allowing unfiltered access to ```inv``
messages <core-ref-p2p-network-data-messages#inv>`__ announcing new
transactions.

Dash Core does not require a ```filterclear``
message <core-ref-p2p-network-control-messages#filterclear>`__ before a
replacement filter is loaded with ``filterload``. It also doesn’t
require a ```filterload``
message <core-ref-p2p-network-control-messages#filterload>`__ before a
```filterclear``
message <core-ref-p2p-network-control-messages#filterclear>`__.

There is no payload in a ```filterclear``
message <core-ref-p2p-network-control-messages#filterclear>`__. See the
`message header section <core-ref-p2p-network-message-headers>`__ for an
example of a message without a payload.

filterload
==========

*Added in protocol version 70001 as described by BIP37.*

The ```filterload``
message <core-ref-p2p-network-control-messages#filterload>`__ tells the
receiving <> to filter all relayed transactions and requested <> through
the provided filter. This allows clients to receive transactions
relevant to their <> plus a configurable rate of false positive
transactions which can provide plausible-deniability privacy.

+-------------+-------------------+--------------+--------------------+
| Bytes       | Name              | Data Type    | Description        |
+=============+===================+==============+====================+
| *Varies*    | nFilterBytes      | compactSize  | Number of bytes in |
|             |                   | uint         | the following      |
|             |                   |              | filter bit field.  |
+-------------+-------------------+--------------+--------------------+
| *Varies*    | filter            | uint8_t[]    | A bit field of     |
|             |                   |              | arbitrary          |
|             |                   |              | byte-aligned size. |
|             |                   |              | The maximum size   |
|             |                   |              | is 36,000 bytes.   |
+-------------+-------------------+--------------+--------------------+
| 4           | nHashFuncs        | uint32_t     | The number of hash |
|             |                   |              | functions to use   |
|             |                   |              | in this filter.    |
|             |                   |              | The maximum value  |
|             |                   |              | allowed in this    |
|             |                   |              | field is 50.       |
+-------------+-------------------+--------------+--------------------+
| 4           | nTweak            | uint32_t     | An arbitrary value |
|             |                   |              | to add to the seed |
|             |                   |              | value in the hash  |
|             |                   |              | function used by   |
|             |                   |              | the bloom filter.  |
+-------------+-------------------+--------------+--------------------+
| 1           | nFlags            | uint8_t      | A set of flags     |
|             |                   |              | that control how   |
|             |                   |              | outpoints          |
|             |                   |              | corresponding to a |
|             |                   |              | matched pubkey     |
|             |                   |              | script are added   |
|             |                   |              | to the filter. See |
|             |                   |              | the table in the   |
|             |                   |              | Updating A Bloom   |
|             |                   |              | Filter subsection  |
|             |                   |              | below.             |
+-------------+-------------------+--------------+--------------------+

The annotated hexdump below shows a ```filterload``
message <core-ref-p2p-network-control-messages#filterload>`__. (The
message header has been omitted.) For an example of how this payload was
created, see the `filterload
example <core-examples-p2p-network-creating-a-bloom-filter>`__.

.. code:: text

   02 ......... Filter bytes: 2
   b50f ....... Filter: 1010 1101 1111 0000
   0b000000 ... nHashFuncs: 11
   00000000 ... nTweak: 0/none
   00 ......... nFlags: BLOOM_UPDATE_NONE

Initializing A Bloom Filter
---------------------------

Filters have two core parameters: the size of the bit field and the
number of hash functions to run against each data element. The following
formulas from BIP37 will allow you to automatically select appropriate
values based on the number of elements you plan to insert into the
filter (*n*) and the false positive rate (*p*) you desire to maintain
plausible deniability.

-  Size of the bit field in bytes (*nFilterBytes*), up to a maximum of
   36,000: ``(-1 / log(2)**2 * n * log(p)) / 8``

-  Hash functions to use (*nHashFuncs*), up to a maximum of 50:
   ``nFilterBytes * 8 / n * log(2)``

Note that the filter matches parts of transactions (transaction
elements), so the false positive rate is relative to the number of
elements checked—not the number of transactions checked. Each normal
transaction has a minimum of four matchable elements (described in the
comparison subsection below), so a filter with a false-positive rate of
1 percent will match about 4 percent of all transactions at a minimum.

According to BIP37, the formulas and limits described above provide
support for bloom filters containing 20,000 items with a false positive
rate of less than 0.1 percent or 10,000 items with a false positive rate
of less than 0.0001 percent.

Once the size of the bit field is known, the bit field should be
initialized as all zeroes.

Populating A Bloom Filter
-------------------------

The bloom filter is populated using between 1 and 50 unique hash
functions (the number specified per filter by the *nHashFuncs* field).
Instead of using up to 50 different hash function implementations, a
single implementation is used with a unique seed value for each
function.

The seed is ``nHashNum * 0xfba4c795 + nTweak`` as a *uint32_t*, where
the values are:

-  **nHashNum** is the sequence number for this hash function, starting
   at 0 for the first hash iteration and increasing up to the value of
   the *nHashFuncs* field (minus one) for the last hash iteration.

-  **0xfba4c795** is a constant optimized to create large differences in
   the seed for different values of *nHashNum*.

-  **nTweak** is a per-filter constant set by the client to require the
   use of an arbitrary set of hash functions.

If the seed resulting from the formula above is larger than four bytes,
it must be truncated to its four most significant bytes (for example,
``0x8967452301 & 0xffffffff → 0x67452301``).

The actual hash function implementation used is the `32-bit Murmur3 hash
function <https://en.wikipedia.org/wiki/MurmurHash>`__. [block:callout]
{ “type”: “warning”, “body”: “**Warning:** the Murmur3 hash function has
separate 32-bit and 64-bit versions that produce different results for
the same <>. Only the 32-bit Murmur3 version is used with Dash bloom
filters.”, “title”: “Murmer3 Version” } [/block] The data to be hashed
can be any transaction element which the bloom filter can match. See the
next subsection for the list of transaction elements checked against the
filter. The largest element which can be matched is a script data push
of 520 bytes, so the data should never exceed 520 bytes.

The example below from Dash Core
`bloom.cpp <https://github.com/dashpay/dash/blob/v0.15.x/src/bloom.cpp#L59>`__
combines all the steps above to create the hash function template. The
seed is the first parameter; the data to be hashed is the second
parameter. The result is a uint32_t modulo the size of the bit field in
bits.

.. code:: cpp

   MurmurHash3(nHashNum * 0xFBA4C795 + nTweak, vDataToHash) % (vData.size() * 8)

Each data element to be added to the filter is hashed by *nHashFuncs*
number of hash functions. Each time a hash function is run, the result
will be the index number (*nIndex*) of a bit in the bit field. That bit
must be set to 1. For example if the filter bit field was ``00000000``
and the result is 5, the revised filter bit field is ``00000100`` (the
first bit is bit 0).

It is expected that sometimes the same index number will be returned
more than once when populating the bit field; this does not affect the
algorithm—after a bit is set to 1, it is never changed back to 0.

After all data elements have been added to the filter, each set of eight
bits is converted into a little-endian byte. These bytes are the value
of the *filter* field.

Comparing Transaction Elements To A Bloom Filter
------------------------------------------------

To compare an arbitrary data element against the bloom filter, it is
hashed using the same parameters used to create the bloom filter.
Specifically, it is hashed *nHashFuncs* times, each time using the same
*nTweak* provided in the filter, and the resulting <> is modulo the size
of the bit field provided in the *filter* field. After each hash is
performed, the filter is checked to see if the bit at that indexed
location is set. For example if the result of a hash is ``5`` and the
filter is ``01001110``, the bit is considered set.

If the result of every hash points to a set bit, the filter matches. If
any of the results points to an unset bit, the filter does not match.

The following transaction elements are compared against bloom filters.
All elements will be hashed in the byte order used in <> (for example,
<> will be in <>).

-  **TXIDs:** the transaction’s SHA256(SHA256()) hash.

-  **Outpoints:** each 36-byte <> used this transaction’s input section
   is individually compared to the filter.

-  **Signature Script Data:** each element pushed onto the stack by a <>
   in a <> from this transaction is individually compared to the filter.
   This includes data elements present in P2SH <> when they are being
   spent.

-  **PubKey Script Data:** each element pushed onto the the stack by a
   data-pushing opcode in any <> from this transaction is individually
   compared to the filter. (If a pubkey script element matches the
   filter, the filter will be immediately updated if the
   ``BLOOM_UPDATE_ALL`` flag was set; if the pubkey script is in the
   P2PKH format and matches the filter, the filter will be immediately
   updated if the ``BLOOM_UPDATE_P2PUBKEY_ONLY`` flag was set. See the
   subsection below for details.)

As of Dash Core 0.14.0, elements in the extra payload section of
`DIP2 <https://github.com/dashpay/dips/blob/master/dip-0002.md>`__-based
<> are also compared against bloom filters.

The following annotated hexdump of a transaction is from the `raw
transaction format
section <core-ref-transactions-raw-transaction-format>`__; the elements
which would be checked by the filter are emphasized in bold. Note that
this transaction’s TXID (**``01000000017b1eab[...]``**) would also be
checked, and that the outpoint TXID and index number below would be
checked as a single 36-byte element.

.. raw:: html

   <pre><code>01000000 ................................... Version

   01 ......................................... Number of inputs
   |
   | <b>7b1eabe0209b1fe794124575ef807057</b>
   | <b>c77ada2138ae4fa8d6c4de0398a14f3f</b> ......... Outpoint TXID
   | <b>00000000</b> ................................. Outpoint index number
   |
   | 49 ....................................... Bytes in sig. script: 73
   | | 48 ..................................... Push 72 bytes as data
   | | | <b>30450221008949f0cb400094ad2b5eb3</b>
   | | | <b>99d59d01c14d73d8fe6e96df1a7150de</b>
   | | | <b>b388ab8935022079656090d7f6bac4c9</b>
   | | | <b>a94e0aad311a4268e082a725f8aeae05</b>
   | | | <b>73fb12ff866a5f01</b> ..................... Secp256k1 signature
   |
   | ffffffff ................................. Sequence number: UINT32_MAX

   01 ......................................... Number of outputs
   | f0ca052a01000000 ......................... Satoshis (49.99990000 BTC)
   |
   | 19 ....................................... Bytes in pubkey script: 25
   | | 76 ..................................... OP_DUP
   | | a9 ..................................... OP_HASH160
   | | 14 ..................................... Push 20 bytes as data
   | | | <b>cbc20a7664f2f69e5355aa427045bc15</b>
   | | | <b>e7c6c772</b> ............................. PubKey hash
   | | 88 ..................................... OP_EQUALVERIFY
   | | ac ..................................... OP_CHECKSIG

   00000000 ................................... locktime: 0 (a block height)
   </code></pre>

Updating A Bloom Filter
-----------------------

Clients will often want to track <> that spend <> (outpoints) relevant
to their wallet, so the filterload field *nFlags* can be set to allow
the filtering <> to update the filter when a match is found. When the
filtering node sees a <> that pays a pubkey, <>, or other data element
matching the filter, the filtering node immediately updates the filter
with the <> corresponding to that pubkey script.

.. figure:: https://dash-docs.github.io/img/dev/en-bloom-update.svg
   :alt: Automatically Updating Bloom Filters

   Automatically Updating Bloom Filters

If an input later spends that outpoint, the filter will match it,
allowing the filtering node to tell the client that one of its
transaction outputs has been spent.

The *nFlags* field has three allowed values:

+---------+---------------------------------------+--------------------+
| Value   | Name                                  | Description        |
+=========+=======================================+====================+
| 0       | ``BLOOM_UPDATE_NONE``                 | The filtering node |
|         |                                       | should not update  |
|         |                                       | the filter.        |
+---------+---------------------------------------+--------------------+
| 1       | ``BLOOM_UPDATE_ALL``                  | If the filter      |
|         |                                       | matches any data   |
|         |                                       | element in a       |
|         |                                       | pubkey script, the |
|         |                                       | corresponding      |
|         |                                       | outpoint is added  |
|         |                                       | to the filter.     |
+---------+---------------------------------------+--------------------+
| 2       | ``BLOOM_UPDATE_P2PUBKEY_ONLY``        | If the filter      |
|         |                                       | matches any data   |
|         |                                       | element in a       |
|         |                                       | pubkey script and  |
|         |                                       | that script is     |
|         |                                       | either a P2PKH or  |
|         |                                       | non-P2SH           |
|         |                                       | pay-to-multisig    |
|         |                                       | script, the        |
|         |                                       | corresponding      |
|         |                                       | outpoint is added  |
|         |                                       | to the filter.     |
+---------+---------------------------------------+--------------------+

In addition, because the filter size stays the same even though
additional elements are being added to it, the false positive rate
increases. Each false positive can result in another element being added
to the filter, creating a feedback loop that can (after a certain point)
make the filter useless. For this reason, clients using automatic filter
updates need to monitor the actual false positive rate and send a new
filter when the rate gets too high.

getaddr
=======

The ```getaddr``
message <core-ref-p2p-network-control-messages#getaddr>`__ requests an
```addr`` message <core-ref-p2p-network-control-messages#addr>`__ from
the receiving <>, preferably one with lots of IP addresses of other
receiving nodes. The transmitting node can use those IP addresses to
quickly update its database of available nodes rather than waiting for
unsolicited ```addr``
messages <core-ref-p2p-network-control-messages#addr>`__ to arrive over
time.

There is no payload in a ```getaddr``
message <core-ref-p2p-network-control-messages#getaddr>`__. See the
`message header section <core-ref-p2p-network-message-headers>`__ for an
example of a message without a payload.

getsporks
=========

The ```getsporks``
message <core-ref-p2p-network-control-messages#getsporks>`__ requests
```spork`` messages <core-ref-p2p-network-control-messages#spork>`__
from the receiving node.

There is no payload in a ```getsporks``
message <core-ref-p2p-network-control-messages#getsporks>`__. See the
`message header section <core-ref-p2p-network-message-headers>`__ for an
example of a message without a payload.

ping
====

The ```ping`` message <core-ref-p2p-network-control-messages#ping>`__
helps confirm that the receiving <> is still connected. If a TCP/IP
error is encountered when sending the ```ping``
message <core-ref-p2p-network-control-messages#ping>`__ (such as a
connection timeout), the transmitting node can assume that the receiving
node is disconnected. The response to a ```ping``
message <core-ref-p2p-network-control-messages#ping>`__ is the ```pong``
message <core-ref-p2p-network-control-messages#pong>`__.

Before protocol version 60000, the ```ping``
message <core-ref-p2p-network-control-messages#ping>`__ had no payload.
As of protocol version 60001 and all later versions, the message
includes a single field, the nonce.

+-----------+-----------+------------------+--------------------------+
| Bytes     | Name      | Data Type        | Description              |
+===========+===========+==================+==========================+
| 8         | nonce     | uint64_t         | *Added in protocol       |
|           |           |                  | version 60001 as         |
|           |           |                  | described by BIP31.*     |
|           |           |                  | Random nonce assigned to |
|           |           |                  | this ```ping``           |
|           |           |                  | message                  |
|           |           |                  | <core-ref-p2p-network-co |
|           |           |                  | ntrol-messages#ping>`__. |
|           |           |                  | The responding ```pong`` |
|           |           |                  | message                  |
|           |           |                  |  <core-ref-p2p-network-c |
|           |           |                  | ontrol-messages#pong>`__ |
|           |           |                  | will include this nonce  |
|           |           |                  | to identify the          |
|           |           |                  | ```ping``                |
|           |           |                  | message                  |
|           |           |                  |  <core-ref-p2p-network-c |
|           |           |                  | ontrol-messages#ping>`__ |
|           |           |                  | to which it is replying. |
+-----------+-----------+------------------+--------------------------+

The annotated hexdump below shows a ```ping``
message <core-ref-p2p-network-control-messages#ping>`__. (The message
header has been omitted.)

.. code:: text

   0094102111e2af4d ... Nonce

pong
====

*Added in protocol version 60001 as described by BIP31.*

The ```pong`` message <core-ref-p2p-network-control-messages#pong>`__
replies to a ```ping``
message <core-ref-p2p-network-control-messages#ping>`__, proving to the
pinging <> that the ponging node is still alive. Dash Core will, by
default, disconnect from any clients which have not responded to a
```ping`` message <core-ref-p2p-network-control-messages#ping>`__ within
20 minutes.

To allow nodes to keep track of latency, the ```pong``
message <core-ref-p2p-network-control-messages#pong>`__ sends back the
same nonce received in the ```ping``
message <core-ref-p2p-network-control-messages#ping>`__ it is replying
to.

The format of the ```pong``
message <core-ref-p2p-network-control-messages#pong>`__ is identical to
the ```ping`` message <core-ref-p2p-network-control-messages#ping>`__;
only the message header differs.

reject
======

*Added in protocol version 70002 as described by BIP61.*

The ```reject``
message <core-ref-p2p-network-control-messages#reject>`__ informs the
receiving <> that one of its previous messages has been rejected.

+-----------+-----------------+---------------------+----------------+
| Bytes     | Name            | Data Type           | Description    |
+===========+=================+=====================+================+
| *Varies*  | message bytes   | compactSize uint    | The number of  |
|           |                 |                     | bytes in the   |
|           |                 |                     | following      |
|           |                 |                     | message field. |
+-----------+-----------------+---------------------+----------------+
| *Varies*  | message         | string              | The type of    |
|           |                 |                     | message        |
|           |                 |                     | rejected as    |
|           |                 |                     | ASCII text     |
|           |                 |                     | *without null  |
|           |                 |                     | padding*. For  |
|           |                 |                     | example: “tx”, |
|           |                 |                     | “block”, or    |
|           |                 |                     | “version”.     |
+-----------+-----------------+---------------------+----------------+
| 1         | code            | char                | The reject     |
|           |                 |                     | message code.  |
|           |                 |                     | See the table  |
|           |                 |                     | below.         |
+-----------+-----------------+---------------------+----------------+
| *Varies*  | reason bytes    | compactSize uint    | The number of  |
|           |                 |                     | bytes in the   |
|           |                 |                     | following      |
|           |                 |                     | reason field.  |
|           |                 |                     | May be 0x00 if |
|           |                 |                     | a text reason  |
|           |                 |                     | isn’t          |
|           |                 |                     | provided.      |
+-----------+-----------------+---------------------+----------------+
| *Varies*  | reason          | string              | The reason for |
|           |                 |                     | the rejection  |
|           |                 |                     | in ASCII text. |
|           |                 |                     | This should    |
|           |                 |                     | not be         |
|           |                 |                     | displayed to   |
|           |                 |                     | the user; it   |
|           |                 |                     | is only for    |
|           |                 |                     | debugging      |
|           |                 |                     | purposes.      |
+-----------+-----------------+---------------------+----------------+
| *Varies*  | extra data      | *varies*            | Optional       |
|           |                 |                     | additional     |
|           |                 |                     | data provided  |
|           |                 |                     | with the       |
|           |                 |                     | rejection. For |
|           |                 |                     | example, most  |
|           |                 |                     | rejections of  |
|           |                 |                     | ```tx``        |
|           |                 |                     | messages       |
|           |                 |                     | <core-ref-p2p- |
|           |                 |                     | network-data-m |
|           |                 |                     | essages#tx>`__ |
|           |                 |                     | or ```block``  |
|           |                 |                     | messages <co   |
|           |                 |                     | re-ref-p2p-net |
|           |                 |                     | work-data-mess |
|           |                 |                     | ages#block>`__ |
|           |                 |                     | include the    |
|           |                 |                     | hash of the    |
|           |                 |                     | rejected       |
|           |                 |                     | transaction or |
|           |                 |                     | block header.  |
|           |                 |                     | See the code   |
|           |                 |                     | table below.   |
+-----------+-----------------+---------------------+----------------+

The following table lists message reject codes. Codes are tied to the
type of message they reply to; for example there is a 0x10 reject code
for transactions and a 0x10 reject code for blocks.

+-----+-------------------+-------------+------------+----------------+
| C   | In Reply To       | Extra Bytes | Extra Type | Description    |
| ode |                   |             |            |                |
+=====+===================+=============+============+================+
| 0   | *any message*     | 0           | N/A        | Message could  |
| x01 |                   |             |            | not be         |
|     |                   |             |            | decoded. Be    |
|     |                   |             |            | careful of     |
|     |                   |             |            | ```reject``    |
|     |                   |             |            | m              |
|     |                   |             |            | essage <core-r |
|     |                   |             |            | ef-p2p-network |
|     |                   |             |            | -control-messa |
|     |                   |             |            | ges#reject>`__ |
|     |                   |             |            | feedback loops |
|     |                   |             |            | where two      |
|     |                   |             |            | peers each     |
|     |                   |             |            | don’t          |
|     |                   |             |            | understand     |
|     |                   |             |            | each other’s   |
|     |                   |             |            | ```reject``    |
|     |                   |             |            | me             |
|     |                   |             |            | ssages <core-r |
|     |                   |             |            | ef-p2p-network |
|     |                   |             |            | -control-messa |
|     |                   |             |            | ges#reject>`__ |
|     |                   |             |            | and so keep    |
|     |                   |             |            | sending them   |
|     |                   |             |            | back and forth |
|     |                   |             |            | forever.       |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```block``        | 32          | char[32]   | Block is       |
| x10 | me                |             |            | invalid for    |
|     | ssage <core-ref-p |             |            | some reason    |
|     | 2p-network-data-m |             |            | (invalid       |
|     | essages#block>`__ |             |            | proof-of-work, |
|     |                   |             |            | invalid        |
|     |                   |             |            | signature,     |
|     |                   |             |            | etc). Extra    |
|     |                   |             |            | data may       |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | block’s header |
|     |                   |             |            | hash.          |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```tx``           | 32          | char[32]   | Transaction is |
| x10 | message <core-re  |             |            | invalid for    |
|     | f-p2p-network-dat |             |            | some reason    |
|     | a-messages#tx>`__ |             |            | (invalid       |
|     |                   |             |            | signature,     |
|     |                   |             |            | output value   |
|     |                   |             |            | greater than   |
|     |                   |             |            | input, etc.).  |
|     |                   |             |            | Extra data may |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | transaction’s  |
|     |                   |             |            | TXID.          |
+-----+-------------------+-------------+------------+----------------+
| 0   | ``ix`` message    | 32          | char[32]   | InstantSend    |
| x10 |                   |             |            | transaction is |
|     |                   |             |            | invalid for    |
|     |                   |             |            | some reason    |
|     |                   |             |            | (invalid tx    |
|     |                   |             |            | lock request,  |
|     |                   |             |            | conflicting tx |
|     |                   |             |            | lock request,  |
|     |                   |             |            | etc.). Extra   |
|     |                   |             |            | data may       |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | transaction’s  |
|     |                   |             |            | TXID.          |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```block``        | 32          | char[32]   | The block uses |
| x11 | me                |             |            | a version that |
|     | ssage <core-ref-p |             |            | is no longer   |
|     | 2p-network-data-m |             |            | supported.     |
|     | essages#block>`__ |             |            | Extra data may |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | block’s header |
|     |                   |             |            | hash.          |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```version``      | 0           | N/A        | Connecting     |
| x11 | message           |             |            | node is using  |
|     |  <core-ref-p2p-ne |             |            | a protocol     |
|     | twork-control-mes |             |            | version that   |
|     | sages#version>`__ |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```dsa``          | 0           | N/A        | Connecting     |
| x11 | message           |             |            | node is using  |
|     |  <core-ref-p2p-ne |             |            | a CoinJoin     |
|     | twork-privatesend |             |            | protocol       |
|     | -messages#dsa>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```dsi``          | 0           | N/A        | Connecting     |
| x11 | message           |             |            | node is using  |
|     |  <core-ref-p2p-ne |             |            | a CoinJoin     |
|     | twork-privatesend |             |            | protocol       |
|     | -messages#dsi>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```dsc``          | 0           | N/A        | Connecting     |
| x11 | message           |             |            | node is using  |
|     |  <core-ref-p2p-ne |             |            | a CoinJoin     |
|     | twork-privatesend |             |            | protocol       |
|     | -messages#dsc>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```dsf``          | 0           | N/A        | Connecting     |
| x11 | message           |             |            | node is using  |
|     |  <core-ref-p2p-ne |             |            | a CoinJoin     |
|     | twork-privatesend |             |            | protocol       |
|     | -messages#dsf>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```dsq``          | 0           | N/A        | Connecting     |
| x11 | message           |             |            | node is using  |
|     |  <core-ref-p2p-ne |             |            | a CoinJoin     |
|     | twork-privatesend |             |            | protocol       |
|     | -messages#dsq>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```dssu``         | 0           | N/A        | Connecting     |
| x11 | message           |             |            | node is using  |
|     | <core-ref-p2p-net |             |            | a CoinJoin     |
|     | work-privatesend- |             |            | protocol       |
|     | messages#dssu>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```govsync``      | 0           | N/A        | Connecting     |
| x11 | message <c        |             |            | node is using  |
|     | ore-ref-p2p-netwo |             |            | a governance   |
|     | rk-governance-mes |             |            | protocol       |
|     | sages#govsync>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```govobj``       | 0           | N/A        | Connecting     |
| x11 | message <         |             |            | node is using  |
|     | core-ref-p2p-netw |             |            | a governance   |
|     | ork-governance-me |             |            | protocol       |
|     | ssages#govobj>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```govobjvote``   | 0           | N/A        | Connecting     |
| x11 | message <core     |             |            | node is using  |
|     | -ref-p2p-network- |             |            | a governance   |
|     | governance-messag |             |            | protocol       |
|     | es#govobjvote>`__ |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ``mnget`` message | 0           | N/A        | Connecting     |
| x11 |                   |             |            | node is using  |
|     |                   |             |            | a masternode   |
|     |                   |             |            | payment        |
|     |                   |             |            | protocol       |
|     |                   |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ``mnw`` message   | 0           | N/A        | Connecting     |
| x11 |                   |             |            | node is using  |
|     |                   |             |            | a masternode   |
|     |                   |             |            | payment        |
|     |                   |             |            | protocol       |
|     |                   |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ``txlvote``       | 0           | N/A        | Connecting     |
| x11 | message           |             |            | node is using  |
|     |                   |             |            | an InstantSend |
|     |                   |             |            | protocol       |
|     |                   |             |            | version that   |
|     |                   |             |            | the rejecting  |
|     |                   |             |            | node considers |
|     |                   |             |            | obsolete and   |
|     |                   |             |            | unsupported.   |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```tx``           | 32          | char[32]   | Duplicate      |
| x12 | message <core-re  |             |            | input spend    |
|     | f-p2p-network-dat |             |            | (double        |
|     | a-messages#tx>`__ |             |            | spend): the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | transaction    |
|     |                   |             |            | spends the     |
|     |                   |             |            | same input as  |
|     |                   |             |            | a              |
|     |                   |             |            | previ          |
|     |                   |             |            | ously-received |
|     |                   |             |            | transaction.   |
|     |                   |             |            | Extra data may |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | transaction’s  |
|     |                   |             |            | TXID.          |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```version``      | 0           | N/A        | More than one  |
| x12 | message           |             |            | ```version``   |
|     |  <core-ref-p2p-ne |             |            | me             |
|     | twork-control-mes |             |            | ssage <core-re |
|     | sages#version>`__ |             |            | f-p2p-network- |
|     |                   |             |            | control-messag |
|     |                   |             |            | es#version>`__ |
|     |                   |             |            | received in    |
|     |                   |             |            | this           |
|     |                   |             |            | connection.    |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```tx``           | 32          | char[32]   | The            |
| x40 | message <core-re  |             |            | transaction    |
|     | f-p2p-network-dat |             |            | will not be    |
|     | a-messages#tx>`__ |             |            | mined or       |
|     |                   |             |            | relayed        |
|     |                   |             |            | because the    |
|     |                   |             |            | rejecting node |
|     |                   |             |            | considers it   |
|     |                   |             |            | non-standard—a |
|     |                   |             |            | transaction    |
|     |                   |             |            | type or        |
|     |                   |             |            | version        |
|     |                   |             |            | unknown by the |
|     |                   |             |            | server. Extra  |
|     |                   |             |            | data may       |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | transaction’s  |
|     |                   |             |            | TXID.          |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```tx``           | 32          | char[32]   | One or more    |
| x41 | message <core-re  |             |            | output amounts |
|     | f-p2p-network-dat |             |            | are below the  |
|     | a-messages#tx>`__ |             |            | dust           |
|     |                   |             |            | threshold.     |
|     |                   |             |            | Extra data may |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | transaction’s  |
|     |                   |             |            | TXID.          |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```tx``           |             | char[32]   | The            |
| x42 | message <core-re  |             |            | transaction    |
|     | f-p2p-network-dat |             |            | did not have a |
|     | a-messages#tx>`__ |             |            | large enough   |
|     |                   |             |            | fee or         |
|     |                   |             |            | priority to be |
|     |                   |             |            | relayed or     |
|     |                   |             |            | mined. Extra   |
|     |                   |             |            | data may       |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | transaction’s  |
|     |                   |             |            | TXID.          |
+-----+-------------------+-------------+------------+----------------+
| 0   | ```block``        | 32          | char[32]   | The block      |
| x43 | me                |             |            | belongs to a   |
|     | ssage <core-ref-p |             |            | block chain    |
|     | 2p-network-data-m |             |            | which is not   |
|     | essages#block>`__ |             |            | the same block |
|     |                   |             |            | chain as       |
|     |                   |             |            | provided by a  |
|     |                   |             |            | compiled-in    |
|     |                   |             |            | checkpoint.    |
|     |                   |             |            | Extra data may |
|     |                   |             |            | include the    |
|     |                   |             |            | rejected       |
|     |                   |             |            | block’s header |
|     |                   |             |            | hash.          |
+-----+-------------------+-------------+------------+----------------+

Reject Codes

==== ================
Code Description
==== ================
0x01 Malformed
0x10 Invalid
0x11 Obsolete
0x12 Duplicate
0x40 Non-standard
0x41 Dust
0x42 Insufficient fee
0x43 Checkpoint
==== ================

The annotated hexdump below shows a ```reject``
message <core-ref-p2p-network-control-messages#reject>`__. (The message
header has been omitted.)

.. code:: text

   02 ................................. Number of bytes in message: 2
   7478 ............................... Type of message rejected: tx
   12 ................................. Reject code: 0x12 (duplicate)
   15 ................................. Number of bytes in reason: 21
   6261642d74786e732d696e707574732d
   7370656e74 ......................... Reason: bad-txns-inputs-spent
   394715fcab51093be7bfca5a31005972
   947baf86a31017939575fb2354222821 ... TXID

sendaddrv2
==========

*Added in protocol version 70220 of Dash Core*

The ``sendaddrv2`` message signals support for receiving ```addrv2``
messages <#addrv2>`__ (defined in
`BIP155 <https://github.com/bitcoin/bips/blob/master/bip-0155.mediawiki>`__).
It also implies that its sender can encode as ``addrv2`` and would send
``addrv2`` messages instead of ```addr`` messages <#addr>`__ to a peer
that has signaled ``addrv2`` support by sending a ``sendaddrv2``
message.

There is no payload in a ``sendaddrv2`` message. See the `message header
section <core-ref-p2p-network-message-headers>`__ for an example of a
message without a payload.

sendcmpct
=========

*Added in protocol version 70209 of Dash Core as described by BIP152*

The ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__ tells the
receiving <> whether or not to announce new <> using a ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__. It also
sends the compact block protocol version it supports. The ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__ is defined
as a message containing a 1-byte integer followed by a 8-byte integer.
The first integer is interpreted as a boolean and should have a value of
either 1 or 0. The second integer is be interpreted as a little-endian
version number.

Upon receipt of a ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__ with the
first and second integers set to 1, the <> should announce new blocks by
sending a ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__.

Upon receipt of a ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__ with the
first integer set to 0, the node shouldn’t announce new blocks by
sending a ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__, but instead
announce new blocks by sending invs or <>, as defined by
`BIP130 <https://github.com/bitcoin/bips/blob/master/bip-0130.mediawiki>`__.

Upon receipt of a ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__ with the
second integer set to something other than 1, nodes should treat the
peer as if they had not received the message (as it indicates the peer
will provide an unexpected encoding in ```cmpctblock``
messages <core-ref-p2p-network-data-messages#cmpctblock>`__, and/or
other, messages). This allows future versions to send duplicate
```sendcmpct``
messages <core-ref-p2p-network-control-messages#sendcmpct>`__ with
different versions as a part of a version handshake.

Nodes should check for a protocol version of >= 70209 before sending
```sendcmpct``
messages <core-ref-p2p-network-control-messages#sendcmpct>`__. Nodes
shouldn’t send a request for a ``MSG_CMPCT_BLOCK`` object to a peer
before having received a ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__ from that
peer. Nodes shouldn’t request a ``MSG_CMPCT_BLOCK`` object before having
sent all ```sendcmpct``
messages <core-ref-p2p-network-control-messages#sendcmpct>`__ to that
peer which they intend to send, as the peer cannot know what protocol
version to use in the response.

The structure of a ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__ is defined
below.

+-----------+-----------------+---------------------+----------------+
| Bytes     | Name            | Data Type           | Description    |
+===========+=================+=====================+================+
| 1         | announce        | bool                | 0 - Announce   |
|           |                 |                     | blocks via     |
|           |                 |                     | ```headers``   |
|           |                 |                     | message <core  |
|           |                 |                     | -ref-p2p-netwo |
|           |                 |                     | rk-data-messag |
|           |                 |                     | es#headers>`__ |
|           |                 |                     | or ```inv``    |
|           |                 |                     | message <cor   |
|           |                 |                     | e-ref-p2p-netw |
|           |                 |                     | ork-data-messa |
|           |                 |                     | ges#inv>`__\ 1 |
|           |                 |                     | - Announce     |
|           |                 |                     | blocks via     |
|           |                 |                     | `              |
|           |                 |                     | ``cmpctblock`` |
|           |                 |                     | me             |
|           |                 |                     | ssage <core-re |
|           |                 |                     | f-p2p-network- |
|           |                 |                     | data-messages# |
|           |                 |                     | cmpctblock>`__ |
+-----------+-----------------+---------------------+----------------+
| 8         | version         | uint64_t            | The compact    |
|           |                 |                     | block protocol |
|           |                 |                     | version number |
+-----------+-----------------+---------------------+----------------+

The annotated hexdump below shows a ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__. (The
message header has been omitted.)

.. code:: text

   01 ................................. Block announce type: Compact Blocks
   0100000000000000 ................... Compact block version: 1

senddsq
=======

*Added in protocol version 70214 of Dash Core*

The ```senddsq``
message <core-ref-p2p-network-control-messages#senddsq>`__ is used to
notify a <> whether or not to send ```dsq``
messages <core-ref-p2p-network-privatesend-messages#dsq>`__. This allows
clients that are not interested in participating in CoinJoin processing
(e.g. mobile <>) to minimize data usage.

+-----------------+-----------------+-----------------+-----------------+
| Bytes           | Name            | Data type       | Description     |
+=================+=================+=================+=================+
| 1               | fSendDSQueue    | bool            | 0 - Notify peer |
|                 |                 |                 | to not send any |
|                 |                 |                 | ```dsq``        |
|                 |                 |                 | me              |
|                 |                 |                 | ssages <core-re |
|                 |                 |                 | f-p2p-network-p |
|                 |                 |                 | rivatesend-mess |
|                 |                 |                 | ages#dsq>`__\ 1 |
|                 |                 |                 | - Notify peer   |
|                 |                 |                 | to send all     |
|                 |                 |                 | ```dsq``        |
|                 |                 |                 | messages <core  |
|                 |                 |                 | -ref-p2p-networ |
|                 |                 |                 | k-privatesend-m |
|                 |                 |                 | essages#dsq>`__ |
+-----------------+-----------------+-----------------+-----------------+

The following annotated hexdump shows a ```senddsq``
message <core-ref-p2p-network-control-messages#senddsq>`__. (The message
header has been omitted.)

.. code:: text

   01 ................................. CoinJoin participation: Enabled (1)

sendheaders
===========

The ```sendheaders``
message <core-ref-p2p-network-control-messages#sendheaders>`__ tells the
receiving <> to send new <> announcements using a ```headers``
message <core-ref-p2p-network-data-messages#headers>`__ rather than an
```inv`` message <core-ref-p2p-network-data-messages#inv>`__.

There is no payload in a ```sendheaders``
message <core-ref-p2p-network-control-messages#sendheaders>`__. See the
`message header section <core-ref-p2p-network-message-headers>`__ for an
example of a message without a payload.

sendheaders2
============

*Added in protocol version 70223 of Dash Core.*

The ```sendheaders2``
message <core-ref-p2p-network-control-messages#sendheaders2>`__ tells
the receiving <> to send new <> announcements using a ```headers2``
message <core-ref-p2p-network-data-messages#headers2>`__ rather than an
```inv`` message <core-ref-p2p-network-data-messages#inv>`__.

There is no payload in a ```sendheaders2``
message <core-ref-p2p-network-control-messages#sendheaders2>`__. See the
`message header section <core-ref-p2p-network-message-headers>`__ for an
example of a message without a payload.

spork
=====

Sporks are a mechanism by which updated code is released to the network,
but not immediately made active (or “enforced”). Enforcement of the
updated code can be activated remotely. Should problems arise, the code
can be deactivated in the same manner, without the need for a
network-wide rollback or client update.

A ```spork`` message <core-ref-p2p-network-control-messages#spork>`__
may be sent in response to a ```getsporks``
message <core-ref-p2p-network-control-messages#getsporks>`__.

The ```spork`` message <core-ref-p2p-network-control-messages#spork>`__
tells the receiving peer the status of the spork defined by the SporkID
field. Upon receiving a <> message, the client must verify the <> before
accepting the spork message as valid.

+--------------+----------------+-------------+-----------+-----------+
| Bytes        | Name           | Data type   | Required  | De        |
|              |                |             |           | scription |
+==============+================+=============+===========+===========+
| 4            | nSporkID       | int         | Required  | ID        |
|              |                |             |           | assigned  |
|              |                |             |           | in        |
|              |                |             |           | spork.h   |
+--------------+----------------+-------------+-----------+-----------+
| 8            | nValue         | int64_t     | Required  | Value     |
|              |                |             |           | assigned  |
|              |                |             |           | to        |
|              |                |             |           | spo       |
|              |                |             |           | rkDefault |
|              |                |             |           | (d        |
|              |                |             |           | isabled): |
|              |                |             |           | ``400     |
|              |                |             |           | 0000000`` |
+--------------+----------------+-------------+-----------+-----------+
| 8            | nTimeSigned    | int64_t     | Required  | Time the  |
|              |                |             |           | spork     |
|              |                |             |           | value was |
|              |                |             |           | signed    |
+--------------+----------------+-------------+-----------+-----------+
| 66           | vchSig         | char[]      | Required  | Length (1 |
|              |                |             |           | byte) +   |
|              |                |             |           | Signature |
|              |                |             |           | (65       |
|              |                |             |           | bytes)    |
+--------------+----------------+-------------+-----------+-----------+

**Active Sporks
(per**\ ```src/spork.h`` <https://github.com/dashpay/dash/blob/v0.17.x/src/spork.h#L24>`__\ **)**

+-----------------+-----------------+----------------+----------------+
| Spork ID        | Num.            | Name           | Description    |
+=================+=================+================+================+
| 10001           | 2               | ``INSTANT      | **Updated in   |
|                 |                 | SEND_ENABLED`` | Dash Core      |
|                 |                 |                | 0              |
|                 |                 |                | .17.0**\ Turns |
|                 |                 |                | InstantSend on |
|                 |                 |                | and off        |
|                 |                 |                | network wide.  |
|                 |                 |                | Also           |
|                 |                 |                | determines if  |
|                 |                 |                | new mempool    |
|                 |                 |                | transactions   |
|                 |                 |                | should be      |
|                 |                 |                | locked or not. |
+-----------------+-----------------+----------------+----------------+
| 10002           | 3               | ``INSTANTSE    | Turns on and   |
|                 |                 | ND_BLOCK_``\ \ | off            |
|                 |                 |  ``FILTERING`` | InstantSend    |
|                 |                 |                | block          |
|                 |                 |                | filtering      |
+-----------------+-----------------+----------------+----------------+
| 10008           | 9               | ``SUPERBL      | Superblocks    |
|                 |                 | OCKS_ENABLED`` | are enabled    |
|                 |                 |                | (10% of the    |
|                 |                 |                | block reward   |
|                 |                 |                | allocated to   |
|                 |                 |                | fund the dash  |
|                 |                 |                | treasury for   |
|                 |                 |                | funding        |
|                 |                 |                | approved       |
|                 |                 |                | proposals)     |
+-----------------+-----------------+----------------+----------------+
| 10016           | 17              | ``SPORK_17_    | Enable         |
|                 |                 | QUORUM_DKG_``\ | long-living    |
|                 |                 |  \ ``ENABLED`` | masternode     |
|                 |                 |                | quorum (LLMQ)  |
|                 |                 |                | distributed    |
|                 |                 |                | key generation |
|                 |                 |                | (DKG). When    |
|                 |                 |                | enabled,       |
|                 |                 |                | simple PoSe    |
|                 |                 |                | scoring and    |
|                 |                 |                | banning is     |
|                 |                 |                | active as      |
|                 |                 |                | well.          |
+-----------------+-----------------+----------------+----------------+
| 10018           | 19              | ``SPORK_19_    | Enable         |
|                 |                 | CHAINLOCKS_``\ | LLMQ-based     |
|                 |                 |  \ ``ENABLED`` | ChainLocks.    |
+-----------------+-----------------+----------------+----------------+
| 10020           | 21              | ``SPORK_21_QU  | **Added in     |
|                 |                 | ORUM_ALL_``\ \ | Dash Core      |
|                 |                 |  ``CONNECTED`` | 0.             |
|                 |                 |                | 16.0**\ Enable |
|                 |                 |                | connections    |
|                 |                 |                | between all    |
|                 |                 |                | masternodes in |
|                 |                 |                | a quorum to    |
|                 |                 |                | optimize the   |
|                 |                 |                | signature      |
|                 |                 |                | recovery       |
|                 |                 |                | process.Note:  |
|                 |                 |                | Prior to Dash  |
|                 |                 |                | Core 0.17.0    |
|                 |                 |                | this spork     |
|                 |                 |                | also enforced  |
|                 |                 |                | `PoSe          |
|                 |                 |                | r              |
|                 |                 |                | equirements <c |
|                 |                 |                | ore-guide-dash |
|                 |                 |                | -features-proo |
|                 |                 |                | f-of-service#d |
|                 |                 |                | istributed-key |
|                 |                 |                | -generation-pa |
|                 |                 |                | rticipation-re |
|                 |                 |                | quirements>`__ |
|                 |                 |                | for            |
|                 |                 |                | masternodes to |
|                 |                 |                | support a      |
|                 |                 |                | minimum        |
|                 |                 |                | protocol       |
|                 |                 |                | version and    |
|                 |                 |                | maintain open  |
|                 |                 |                | ports.         |
+-----------------+-----------------+----------------+----------------+
| 10022           | 23              | ``SPORK_23_QU  | **Added in     |
|                 |                 | ORUM_POSE``\ \ | Dash Core      |
|                 |                 |  ``CONNECTED`` | 0.1            |
|                 |                 |                | 7.0**\ Enforce |
|                 |                 |                | `PoSe          |
|                 |                 |                | r              |
|                 |                 |                | equirements <c |
|                 |                 |                | ore-guide-dash |
|                 |                 |                | -features-proo |
|                 |                 |                | f-of-service#d |
|                 |                 |                | istributed-key |
|                 |                 |                | -generation-pa |
|                 |                 |                | rticipation-re |
|                 |                 |                | quirements>`__ |
|                 |                 |                | for            |
|                 |                 |                | masternodes to |
|                 |                 |                | support a      |
|                 |                 |                | minimum        |
|                 |                 |                | protocol       |
|                 |                 |                | version and    |
|                 |                 |                | maintain open  |
|                 |                 |                | ports.         |
+-----------------+-----------------+----------------+----------------+

[block:callout] { “type”: “info”, “title”: “Spork 2 Values”, “body”: “As
of Dash Core 0.17.0, spork 2 supports two different enabled
values::raw-latex:`\n `- ``0`` - Masternodes create locks for all
transactions:raw-latex:`\n `- ``1`` - Masternodes only create locks for
transactions included in a block. Transactions in the mempool are not
locked.:raw-latex:`\n`:raw-latex:`\nMode 1` is only for use in the event
of a sustained overload of InstantSend to ensure that ChainLocks can
remain operational. See `PR
4024 <https://github.com/dashpay/dash/pull/4024>`__ for details.” }
[/block]

[block:callout] { “type”: “info”, “title”: “Spork 21 and 23 Values”,
“body”: “Spork 21 supports two different enabled
values::raw-latex:`\n `- ``0`` - Connections established between each
masternode in a quorum (regardless of quorum size):raw-latex:`\n `-
``1`` - The spork is considered active for the llmq which have member
size less than 100.” } [/block] **Removed Sporks** The following sporks
were used in the past but are no longer necessary and have been removed
recently. To see sporks removed longer ago, please see the `previous
version of
documentation <https://dashcore.readme.io/v0.16.0/docs/core-ref-p2p-network-control-messages#spork>`__.
[block:callout] { “type”: “info”, “title”: “Spork 6”, “body”: “Since
spork 6 was never enabled on mainnet, it was removed in Dash Core
0.16.0. The associated logic was hardened in `PR
3662 <https://github.com/dashpay/dash/pull/3662>`__ to support testnet
(where it is enabled). If testnet is reset at some point in the future,
the remaining logic will be removed.” } [/block] \| Spork ID \| Num. \|
Name \| Description \| \| :———-: \| :———-: \| ———– \| ———– \| \| *10004*
\| *5* \| ``INSTANTSEND_MAX_VALUE`` \| *Removed in Dash Core
0.15.0.Controls the max value for an InstantSend transaction (currently
2000 DASH)* \| *10005* \| *6* \| ``NEW_SIGS`` \| *Removed in Dash Core
0.16.0.Turns on and off new signature format for Dash-specific messages*
\| *10011* \| *12* \| ``RECONSIDER_BLOCKS`` \| *Removed in Dash Core
0.15.0.Forces reindex of a specified number of blocks to recover from
unintentional network forks* \| *10014* \| *15* \|
``DETERMINISTIC_MNS_``\ \ ``ENABLED`` \| *Removed in Dash Core
0.16.0.Deterministic masternode lists are enabled* \| *10015* \| *16* \|
``INSTANTSEND_AUTOLOCKS`` \| *Removed in Dash Core 0.16.0.Automatic
InstantSend for transactions with <=4 inputs (also eliminates the
special InstantSend fee requirement for these transactions)* \| *10019*
\| *20* \| ``SPORK_20_INSTANTSEND_``\ \ ``LLMQ_BASED`` \| *Removed in
Dash Core 0.16.0.Enable LLMQ-based InstantSend.* \| *10021* \| *22* \|
``SPORK_22_PS_MORE_``\ \ ``PARTICIPANTS`` \| *Removed in Dash Core
0.17.0*\ \ *Increase the maximum number of participants in CoinJoin
sessions.*

To verify ``vchSig``, compare the hard-coded spork public key
(``strSporkPubKey`` from
```src/chainparams.cpp`` <https://github.com/dashpay/dash/blob/eaf90b77177efbaf9cbed46e822f0d794f1a0ee5/src/chainparams.cpp#L158>`__)
with the public key recovered from the ```spork``
message <core-ref-p2p-network-control-messages#spork>`__\ ’s hash and
``vchSig`` value (implementation details for Dash Core can be found in
``CPubKey::RecoverCompact``). The hash is a double SHA-256 hash of:

-  The spork magic message (``"DarkCoin Signed Message:\n"``)
-  nSporkID + nValue + nTimeSigned

+-----------------------------------+-----------------------------------+
| Network                           | Spork Pubkey (wrapped)            |
+===================================+===================================+
| Mainnet                           | 04549ac134f694c0243f503e8c8a9a9   |
|                                   | 86f5de6610049c40b07816809b0d1d06a |
|                                   | 21b07be27b9bb555931773f62ba6cf35a |
|                                   | 25fd52f694d4e1106ccd237a7bb899fdd |
+-----------------------------------+-----------------------------------+
| Testnet3                          | 046f78dcf911fbd61910136f7f0f8d9   |
|                                   | 0578f68d0b3ac973b5040fb7afb501b59 |
|                                   | 39f39b108b0569dca71488f5bbf498d92 |
|                                   | e4d1194f6f941307ffd95f75e76869f0e |
+-----------------------------------+-----------------------------------+
| RegTest                           | Undefined                         |
+-----------------------------------+-----------------------------------+
| Devnets                           | 046f78dcf911fbd61910136f7f0f8d9   |
|                                   | 0578f68d0b3ac973b5040fb7afb501b59 |
|                                   | 39f39b108b0569dca71488f5bbf498d92 |
|                                   | e4d1194f6f941307ffd95f75e76869f0e |
+-----------------------------------+-----------------------------------+

The following annotated hexdump shows a ```spork``
message <core-ref-p2p-network-control-messages#spork>`__.

.. code:: text

   11270000 .................................... Spork ID: Spork 2 InstantSend enabled (10001)
   0000000000000000 ............................ Value (0)
   2478da5900000000 ............................ Epoch time: 2017-10-08 19:10:28 UTC (1507489828)

   41 .......................................... Signature length: 65

   1b6762d3e70890b5cfaed5d1fd72121c
   d32020c827a89f8128a00acd210f4ea4
   1b36c26c3767f8a24f48663e189865ed
   403ed1e850cdb4207cdd466419d9d183
   45 .......................................... Masternode Signature

verack
======

The ```verack``
message <core-ref-p2p-network-control-messages#verack>`__ acknowledges a
previously-received ```version``
message <core-ref-p2p-network-control-messages#version>`__, informing
the connecting <> that it can begin to send other messages. The
```verack`` message <core-ref-p2p-network-control-messages#verack>`__
has no payload; for an example of a message with no payload, see the
`message headers section <core-ref-p2p-network-message-headers>`__.

version
=======

The ```version``
message <core-ref-p2p-network-control-messages#version>`__ provides
information about the transmitting <> to the receiving node at the
beginning of a connection. Until both <> have exchanged ```version``
messages <core-ref-p2p-network-control-messages#version>`__, no other
messages will be accepted.

If a ```version``
message <core-ref-p2p-network-control-messages#version>`__ is accepted,
the receiving node should send a ```verack``
message <core-ref-p2p-network-control-messages#verack>`__—but no node
should send a ```verack``
message <core-ref-p2p-network-control-messages#verack>`__ before
initializing its half of the connection by first sending a ```version``
message <core-ref-p2p-network-control-messages#version>`__.

Protocol version 70214 added a <> authentication (challenge/response)
system. Following the ```verack``
message <core-ref-p2p-network-control-messages#verack>`__, masternodes
should send a ```mnauth``
message <core-ref-p2p-network-masternode-messages#mnauth>`__ that signs
the ``mnauth_challenge`` with their BLS operator key.

+-----+--------------+-----------+---------------------------+-------+
| By  | Name         | DataType  | Required/Optional         | D     |
| tes |              |           |                           | escri |
|     |              |           |                           | ption |
+=====+==============+===========+===========================+=======+
| 4   | version      | int32_t   | Required                  | The   |
|     |              |           |                           | hi    |
|     |              |           |                           | ghest |
|     |              |           |                           | pro   |
|     |              |           |                           | tocol |
|     |              |           |                           | ve    |
|     |              |           |                           | rsion |
|     |              |           |                           | under |
|     |              |           |                           | stood |
|     |              |           |                           | by    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | node. |
|     |              |           |                           | See   |
|     |              |           |                           | the   |
|     |              |           |                           | `pro  |
|     |              |           |                           | tocol |
|     |              |           |                           | ve    |
|     |              |           |                           | rsion |
|     |              |           |                           | se    |
|     |              |           |                           | ction |
|     |              |           |                           |  <cor |
|     |              |           |                           | e-ref |
|     |              |           |                           | -p2p- |
|     |              |           |                           | netwo |
|     |              |           |                           | rk-pr |
|     |              |           |                           | otoco |
|     |              |           |                           | l-ver |
|     |              |           |                           | sions |
|     |              |           |                           | >`__. |
+-----+--------------+-----------+---------------------------+-------+
| 8   | services     | uint64_t  | Required                  | The   |
|     |              |           |                           | ser   |
|     |              |           |                           | vices |
|     |              |           |                           | supp  |
|     |              |           |                           | orted |
|     |              |           |                           | by    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | node  |
|     |              |           |                           | en    |
|     |              |           |                           | coded |
|     |              |           |                           | as a  |
|     |              |           |                           | bitf  |
|     |              |           |                           | ield. |
|     |              |           |                           | See   |
|     |              |           |                           | the   |
|     |              |           |                           | list  |
|     |              |           |                           | of    |
|     |              |           |                           | se    |
|     |              |           |                           | rvice |
|     |              |           |                           | codes |
|     |              |           |                           | b     |
|     |              |           |                           | elow. |
+-----+--------------+-----------+---------------------------+-------+
| 8   | timestamp    | int64_t   | Required                  | The   |
|     |              |           |                           | cu    |
|     |              |           |                           | rrent |
|     |              |           |                           | Unix  |
|     |              |           |                           | epoch |
|     |              |           |                           | time  |
|     |              |           |                           | acco  |
|     |              |           |                           | rding |
|     |              |           |                           | to    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | n     |
|     |              |           |                           | ode’s |
|     |              |           |                           | c     |
|     |              |           |                           | lock. |
|     |              |           |                           | Be    |
|     |              |           |                           | cause |
|     |              |           |                           | nodes |
|     |              |           |                           | will  |
|     |              |           |                           | r     |
|     |              |           |                           | eject |
|     |              |           |                           | b     |
|     |              |           |                           | locks |
|     |              |           |                           | with  |
|     |              |           |                           | times |
|     |              |           |                           | tamps |
|     |              |           |                           | more  |
|     |              |           |                           | than  |
|     |              |           |                           | two   |
|     |              |           |                           | hours |
|     |              |           |                           | in    |
|     |              |           |                           | the   |
|     |              |           |                           | fu    |
|     |              |           |                           | ture, |
|     |              |           |                           | this  |
|     |              |           |                           | field |
|     |              |           |                           | can   |
|     |              |           |                           | help  |
|     |              |           |                           | other |
|     |              |           |                           | nodes |
|     |              |           |                           | to    |
|     |              |           |                           | dete  |
|     |              |           |                           | rmine |
|     |              |           |                           | that  |
|     |              |           |                           | their |
|     |              |           |                           | clock |
|     |              |           |                           | is    |
|     |              |           |                           | w     |
|     |              |           |                           | rong. |
+-----+--------------+-----------+---------------------------+-------+
| 8   | addr_recv    | uint64_t  | Required                  | *     |
|     | services     |           |                           | Added |
|     |              |           |                           | in    |
|     |              |           |                           | pro   |
|     |              |           |                           | tocol |
|     |              |           |                           | ve    |
|     |              |           |                           | rsion |
|     |              |           |                           | 106.* |
|     |              |           |                           | The   |
|     |              |           |                           | ser   |
|     |              |           |                           | vices |
|     |              |           |                           | supp  |
|     |              |           |                           | orted |
|     |              |           |                           | by    |
|     |              |           |                           | the   |
|     |              |           |                           | rece  |
|     |              |           |                           | iving |
|     |              |           |                           | node  |
|     |              |           |                           | as    |
|     |              |           |                           | perc  |
|     |              |           |                           | eived |
|     |              |           |                           | by    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | node. |
|     |              |           |                           | Same  |
|     |              |           |                           | f     |
|     |              |           |                           | ormat |
|     |              |           |                           | as    |
|     |              |           |                           | the   |
|     |              |           |                           | ‘serv |
|     |              |           |                           | ices’ |
|     |              |           |                           | field |
|     |              |           |                           | a     |
|     |              |           |                           | bove. |
|     |              |           |                           | Dash  |
|     |              |           |                           | Core  |
|     |              |           |                           | will  |
|     |              |           |                           | at    |
|     |              |           |                           | tempt |
|     |              |           |                           | to    |
|     |              |           |                           | pr    |
|     |              |           |                           | ovide |
|     |              |           |                           | acc   |
|     |              |           |                           | urate |
|     |              |           |                           | in    |
|     |              |           |                           | forma |
|     |              |           |                           | tion. |
+-----+--------------+-----------+---------------------------+-------+
| 16  | addr_recv IP | char      | Required                  | *     |
|     | address      |           |                           | Added |
|     |              |           |                           | in    |
|     |              |           |                           | pro   |
|     |              |           |                           | tocol |
|     |              |           |                           | ve    |
|     |              |           |                           | rsion |
|     |              |           |                           | 106.* |
|     |              |           |                           | The   |
|     |              |           |                           | IPv6  |
|     |              |           |                           | ad    |
|     |              |           |                           | dress |
|     |              |           |                           | of    |
|     |              |           |                           | the   |
|     |              |           |                           | rece  |
|     |              |           |                           | iving |
|     |              |           |                           | node  |
|     |              |           |                           | as    |
|     |              |           |                           | perc  |
|     |              |           |                           | eived |
|     |              |           |                           | by    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | node  |
|     |              |           |                           | in    |
|     |              |           |                           | **big |
|     |              |           |                           | e     |
|     |              |           |                           | ndian |
|     |              |           |                           | byte  |
|     |              |           |                           | ord   |
|     |              |           |                           | er**. |
|     |              |           |                           | IPv4  |
|     |              |           |                           | addr  |
|     |              |           |                           | esses |
|     |              |           |                           | can   |
|     |              |           |                           | be    |
|     |              |           |                           | pro   |
|     |              |           |                           | vided |
|     |              |           |                           | as    |
|     |              |           |                           | `I    |
|     |              |           |                           | Pv4-m |
|     |              |           |                           | apped |
|     |              |           |                           | IPv6  |
|     |              |           |                           | a     |
|     |              |           |                           | ddres |
|     |              |           |                           | ses < |
|     |              |           |                           | http: |
|     |              |           |                           | //en. |
|     |              |           |                           | wikip |
|     |              |           |                           | edia. |
|     |              |           |                           | org/w |
|     |              |           |                           | iki/I |
|     |              |           |                           | Pv6#I |
|     |              |           |                           | Pv4-m |
|     |              |           |                           | apped |
|     |              |           |                           | _IPv6 |
|     |              |           |                           | _addr |
|     |              |           |                           | esses |
|     |              |           |                           | >`__. |
|     |              |           |                           | Dash  |
|     |              |           |                           | Core  |
|     |              |           |                           | will  |
|     |              |           |                           | at    |
|     |              |           |                           | tempt |
|     |              |           |                           | to    |
|     |              |           |                           | pr    |
|     |              |           |                           | ovide |
|     |              |           |                           | acc   |
|     |              |           |                           | urate |
|     |              |           |                           | in    |
|     |              |           |                           | forma |
|     |              |           |                           | tion. |
+-----+--------------+-----------+---------------------------+-------+
| 2   | addr_recv    | uint16_t  | Required                  | *     |
|     | port         |           |                           | Added |
|     |              |           |                           | in    |
|     |              |           |                           | pro   |
|     |              |           |                           | tocol |
|     |              |           |                           | ve    |
|     |              |           |                           | rsion |
|     |              |           |                           | 106.* |
|     |              |           |                           | The   |
|     |              |           |                           | port  |
|     |              |           |                           | n     |
|     |              |           |                           | umber |
|     |              |           |                           | of    |
|     |              |           |                           | the   |
|     |              |           |                           | rece  |
|     |              |           |                           | iving |
|     |              |           |                           | node  |
|     |              |           |                           | as    |
|     |              |           |                           | perc  |
|     |              |           |                           | eived |
|     |              |           |                           | by    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | node  |
|     |              |           |                           | in    |
|     |              |           |                           | **big |
|     |              |           |                           | e     |
|     |              |           |                           | ndian |
|     |              |           |                           | byte  |
|     |              |           |                           | ord   |
|     |              |           |                           | er**. |
+-----+--------------+-----------+---------------------------+-------+
| 8   | addr_trans   | uint64_t  | Required                  | The   |
|     | services     |           |                           | ser   |
|     |              |           |                           | vices |
|     |              |           |                           | supp  |
|     |              |           |                           | orted |
|     |              |           |                           | by    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | node. |
|     |              |           |                           | S     |
|     |              |           |                           | hould |
|     |              |           |                           | be    |
|     |              |           |                           | iden  |
|     |              |           |                           | tical |
|     |              |           |                           | to    |
|     |              |           |                           | the   |
|     |              |           |                           | ‘serv |
|     |              |           |                           | ices’ |
|     |              |           |                           | field |
|     |              |           |                           | a     |
|     |              |           |                           | bove. |
+-----+--------------+-----------+---------------------------+-------+
| 16  | addr_trans   | char      | Required                  | The   |
|     | IP address   |           |                           | IPv6  |
|     |              |           |                           | ad    |
|     |              |           |                           | dress |
|     |              |           |                           | of    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | node  |
|     |              |           |                           | in    |
|     |              |           |                           | **big |
|     |              |           |                           | e     |
|     |              |           |                           | ndian |
|     |              |           |                           | byte  |
|     |              |           |                           | ord   |
|     |              |           |                           | er**. |
|     |              |           |                           | IPv4  |
|     |              |           |                           | addr  |
|     |              |           |                           | esses |
|     |              |           |                           | can   |
|     |              |           |                           | be    |
|     |              |           |                           | pro   |
|     |              |           |                           | vided |
|     |              |           |                           | as    |
|     |              |           |                           | `I    |
|     |              |           |                           | Pv4-m |
|     |              |           |                           | apped |
|     |              |           |                           | IPv6  |
|     |              |           |                           | a     |
|     |              |           |                           | ddres |
|     |              |           |                           | ses < |
|     |              |           |                           | http: |
|     |              |           |                           | //en. |
|     |              |           |                           | wikip |
|     |              |           |                           | edia. |
|     |              |           |                           | org/w |
|     |              |           |                           | iki/I |
|     |              |           |                           | Pv6#I |
|     |              |           |                           | Pv4-m |
|     |              |           |                           | apped |
|     |              |           |                           | _IPv6 |
|     |              |           |                           | _addr |
|     |              |           |                           | esses |
|     |              |           |                           | >`__. |
|     |              |           |                           | Set   |
|     |              |           |                           | to    |
|     |              |           |                           | :     |
|     |              |           |                           | :ffff |
|     |              |           |                           | :127. |
|     |              |           |                           | 0.0.1 |
|     |              |           |                           | if    |
|     |              |           |                           | unk   |
|     |              |           |                           | nown. |
+-----+--------------+-----------+---------------------------+-------+
| 2   | addr_trans   | uint16_t  | Required                  | The   |
|     | port         |           |                           | port  |
|     |              |           |                           | n     |
|     |              |           |                           | umber |
|     |              |           |                           | of    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | node  |
|     |              |           |                           | in    |
|     |              |           |                           | **big |
|     |              |           |                           | e     |
|     |              |           |                           | ndian |
|     |              |           |                           | byte  |
|     |              |           |                           | ord   |
|     |              |           |                           | er**. |
+-----+--------------+-----------+---------------------------+-------+
| 8   | nonce        | uint64_t  | Required                  | A     |
|     |              |           |                           | r     |
|     |              |           |                           | andom |
|     |              |           |                           | nonce |
|     |              |           |                           | which |
|     |              |           |                           | can   |
|     |              |           |                           | help  |
|     |              |           |                           | a     |
|     |              |           |                           | node  |
|     |              |           |                           | d     |
|     |              |           |                           | etect |
|     |              |           |                           | a     |
|     |              |           |                           | conne |
|     |              |           |                           | ction |
|     |              |           |                           | to    |
|     |              |           |                           | it    |
|     |              |           |                           | self. |
|     |              |           |                           | If    |
|     |              |           |                           | the   |
|     |              |           |                           | nonce |
|     |              |           |                           | is 0, |
|     |              |           |                           | the   |
|     |              |           |                           | nonce |
|     |              |           |                           | field |
|     |              |           |                           | is    |
|     |              |           |                           | ign   |
|     |              |           |                           | ored. |
|     |              |           |                           | If    |
|     |              |           |                           | the   |
|     |              |           |                           | nonce |
|     |              |           |                           | is    |
|     |              |           |                           | any   |
|     |              |           |                           | thing |
|     |              |           |                           | else, |
|     |              |           |                           | a     |
|     |              |           |                           | node  |
|     |              |           |                           | s     |
|     |              |           |                           | hould |
|     |              |           |                           | term  |
|     |              |           |                           | inate |
|     |              |           |                           | the   |
|     |              |           |                           | conne |
|     |              |           |                           | ction |
|     |              |           |                           | on    |
|     |              |           |                           | re    |
|     |              |           |                           | ceipt |
|     |              |           |                           | of a  |
|     |              |           |                           | ``    |
|     |              |           |                           | `vers |
|     |              |           |                           | ion`` |
|     |              |           |                           | mes   |
|     |              |           |                           | sage  |
|     |              |           |                           | <core |
|     |              |           |                           | -ref- |
|     |              |           |                           | p2p-n |
|     |              |           |                           | etwor |
|     |              |           |                           | k-con |
|     |              |           |                           | trol- |
|     |              |           |                           | messa |
|     |              |           |                           | ges#v |
|     |              |           |                           | ersio |
|     |              |           |                           | n>`__ |
|     |              |           |                           | with  |
|     |              |           |                           | a     |
|     |              |           |                           | nonce |
|     |              |           |                           | it    |
|     |              |           |                           | previ |
|     |              |           |                           | ously |
|     |              |           |                           | sent. |
+-----+--------------+-----------+---------------------------+-------+
| *V  | user_agent   | co        | Required                  | N     |
| ari | bytes        | mpactSize |                           | umber |
| es* |              | uint      |                           | of    |
|     |              |           |                           | bytes |
|     |              |           |                           | in    |
|     |              |           |                           | foll  |
|     |              |           |                           | owing |
|     |              |           |                           | user_ |
|     |              |           |                           | agent |
|     |              |           |                           | f     |
|     |              |           |                           | ield. |
|     |              |           |                           | If    |
|     |              |           |                           | 0x00, |
|     |              |           |                           | no    |
|     |              |           |                           | user  |
|     |              |           |                           | agent |
|     |              |           |                           | field |
|     |              |           |                           | is    |
|     |              |           |                           | sent. |
+-----+--------------+-----------+---------------------------+-------+
| *V  | user_agent   | string    | Required if user_agent    | *Re   |
| ari |              |           | bytes > 0                 | named |
| es* |              |           |                           | in    |
|     |              |           |                           | pro   |
|     |              |           |                           | tocol |
|     |              |           |                           | ve    |
|     |              |           |                           | rsion |
|     |              |           |                           | 60    |
|     |              |           |                           | 000.* |
|     |              |           |                           | User  |
|     |              |           |                           | agent |
|     |              |           |                           | as    |
|     |              |           |                           | de    |
|     |              |           |                           | fined |
|     |              |           |                           | by    |
|     |              |           |                           | B     |
|     |              |           |                           | IP14. |
|     |              |           |                           | Previ |
|     |              |           |                           | ously |
|     |              |           |                           | c     |
|     |              |           |                           | alled |
|     |              |           |                           | s     |
|     |              |           |                           | ubVer |
|     |              |           |                           | .Dash |
|     |              |           |                           | Core  |
|     |              |           |                           | l     |
|     |              |           |                           | imits |
|     |              |           |                           | the   |
|     |              |           |                           | l     |
|     |              |           |                           | ength |
|     |              |           |                           | to    |
|     |              |           |                           | 256   |
|     |              |           |                           | c     |
|     |              |           |                           | harac |
|     |              |           |                           | ters. |
+-----+--------------+-----------+---------------------------+-------+
| 4   | start_height | int32_t   | Required                  | The   |
|     |              |           |                           | h     |
|     |              |           |                           | eight |
|     |              |           |                           | of    |
|     |              |           |                           | the   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansmi |
|     |              |           |                           | tting |
|     |              |           |                           | n     |
|     |              |           |                           | ode’s |
|     |              |           |                           | best  |
|     |              |           |                           | block |
|     |              |           |                           | chain |
|     |              |           |                           | or,   |
|     |              |           |                           | in    |
|     |              |           |                           | the   |
|     |              |           |                           | case  |
|     |              |           |                           | of an |
|     |              |           |                           | SPV   |
|     |              |           |                           | cl    |
|     |              |           |                           | ient, |
|     |              |           |                           | best  |
|     |              |           |                           | block |
|     |              |           |                           | h     |
|     |              |           |                           | eader |
|     |              |           |                           | c     |
|     |              |           |                           | hain. |
+-----+--------------+-----------+---------------------------+-------+
| 1   | relay        | bool      | Optional                  | *     |
|     |              |           |                           | Added |
|     |              |           |                           | in    |
|     |              |           |                           | pro   |
|     |              |           |                           | tocol |
|     |              |           |                           | ve    |
|     |              |           |                           | rsion |
|     |              |           |                           | 70001 |
|     |              |           |                           | as    |
|     |              |           |                           | desc  |
|     |              |           |                           | ribed |
|     |              |           |                           | by    |
|     |              |           |                           | BI    |
|     |              |           |                           | P37.* |
|     |              |           |                           | T     |
|     |              |           |                           | ransa |
|     |              |           |                           | ction |
|     |              |           |                           | relay |
|     |              |           |                           | flag. |
|     |              |           |                           | If    |
|     |              |           |                           | 0x00, |
|     |              |           |                           | no    |
|     |              |           |                           | ```   |
|     |              |           |                           | inv`` |
|     |              |           |                           | me    |
|     |              |           |                           | ssage |
|     |              |           |                           | s <co |
|     |              |           |                           | re-re |
|     |              |           |                           | f-p2p |
|     |              |           |                           | -netw |
|     |              |           |                           | ork-d |
|     |              |           |                           | ata-m |
|     |              |           |                           | essag |
|     |              |           |                           | es#in |
|     |              |           |                           | v>`__ |
|     |              |           |                           | or    |
|     |              |           |                           | ``    |
|     |              |           |                           | `tx`` |
|     |              |           |                           | m     |
|     |              |           |                           | essag |
|     |              |           |                           | es <c |
|     |              |           |                           | ore-r |
|     |              |           |                           | ef-p2 |
|     |              |           |                           | p-net |
|     |              |           |                           | work- |
|     |              |           |                           | data- |
|     |              |           |                           | messa |
|     |              |           |                           | ges#t |
|     |              |           |                           | x>`__ |
|     |              |           |                           | annou |
|     |              |           |                           | ncing |
|     |              |           |                           | new   |
|     |              |           |                           | tr    |
|     |              |           |                           | ansac |
|     |              |           |                           | tions |
|     |              |           |                           | s     |
|     |              |           |                           | hould |
|     |              |           |                           | be    |
|     |              |           |                           | sent  |
|     |              |           |                           | to    |
|     |              |           |                           | this  |
|     |              |           |                           | c     |
|     |              |           |                           | lient |
|     |              |           |                           | until |
|     |              |           |                           | it    |
|     |              |           |                           | sends |
|     |              |           |                           | a     |
|     |              |           |                           | ```fi |
|     |              |           |                           | lterl |
|     |              |           |                           | oad`` |
|     |              |           |                           | m     |
|     |              |           |                           | essag |
|     |              |           |                           | e <co |
|     |              |           |                           | re-re |
|     |              |           |                           | f-p2p |
|     |              |           |                           | -netw |
|     |              |           |                           | ork-c |
|     |              |           |                           | ontro |
|     |              |           |                           | l-mes |
|     |              |           |                           | sages |
|     |              |           |                           | #filt |
|     |              |           |                           | erloa |
|     |              |           |                           | d>`__ |
|     |              |           |                           | or    |
|     |              |           |                           | `     |
|     |              |           |                           | ``fil |
|     |              |           |                           | tercl |
|     |              |           |                           | ear`` |
|     |              |           |                           | mes   |
|     |              |           |                           | sage  |
|     |              |           |                           | <core |
|     |              |           |                           | -ref- |
|     |              |           |                           | p2p-n |
|     |              |           |                           | etwor |
|     |              |           |                           | k-con |
|     |              |           |                           | trol- |
|     |              |           |                           | messa |
|     |              |           |                           | ges#f |
|     |              |           |                           | ilter |
|     |              |           |                           | clear |
|     |              |           |                           | >`__. |
|     |              |           |                           | If    |
|     |              |           |                           | the   |
|     |              |           |                           | relay |
|     |              |           |                           | field |
|     |              |           |                           | is    |
|     |              |           |                           | not   |
|     |              |           |                           | pr    |
|     |              |           |                           | esent |
|     |              |           |                           | or is |
|     |              |           |                           | set   |
|     |              |           |                           | to    |
|     |              |           |                           | 0x01, |
|     |              |           |                           | this  |
|     |              |           |                           | node  |
|     |              |           |                           | wants |
|     |              |           |                           | ```   |
|     |              |           |                           | inv`` |
|     |              |           |                           | me    |
|     |              |           |                           | ssage |
|     |              |           |                           | s <co |
|     |              |           |                           | re-re |
|     |              |           |                           | f-p2p |
|     |              |           |                           | -netw |
|     |              |           |                           | ork-d |
|     |              |           |                           | ata-m |
|     |              |           |                           | essag |
|     |              |           |                           | es#in |
|     |              |           |                           | v>`__ |
|     |              |           |                           | and   |
|     |              |           |                           | ``    |
|     |              |           |                           | `tx`` |
|     |              |           |                           | m     |
|     |              |           |                           | essag |
|     |              |           |                           | es <c |
|     |              |           |                           | ore-r |
|     |              |           |                           | ef-p2 |
|     |              |           |                           | p-net |
|     |              |           |                           | work- |
|     |              |           |                           | data- |
|     |              |           |                           | messa |
|     |              |           |                           | ges#t |
|     |              |           |                           | x>`__ |
|     |              |           |                           | annou |
|     |              |           |                           | ncing |
|     |              |           |                           | new   |
|     |              |           |                           | tra   |
|     |              |           |                           | nsact |
|     |              |           |                           | ions. |
+-----+--------------+-----------+---------------------------+-------+
| 32  | mnaut        | uint256   | Optional                  | *     |
|     | h\_challenge |           |                           | Added |
|     |              |           |                           | in    |
|     |              |           |                           | pro   |
|     |              |           |                           | tocol |
|     |              |           |                           | ve    |
|     |              |           |                           | rsion |
|     |              |           |                           | 7     |
|     |              |           |                           | 0214* |
|     |              |           |                           | A     |
|     |              |           |                           | chal  |
|     |              |           |                           | lenge |
|     |              |           |                           | to be |
|     |              |           |                           | s     |
|     |              |           |                           | igned |
|     |              |           |                           | by    |
|     |              |           |                           | the   |
|     |              |           |                           | rece  |
|     |              |           |                           | iving |
|     |              |           |                           | m     |
|     |              |           |                           | aster |
|     |              |           |                           | node. |
|     |              |           |                           | The   |
|     |              |           |                           | res   |
|     |              |           |                           | ponse |
|     |              |           |                           | is    |
|     |              |           |                           | ret   |
|     |              |           |                           | urned |
|     |              |           |                           | via a |
|     |              |           |                           | `     |
|     |              |           |                           | ``mna |
|     |              |           |                           | uth`` |
|     |              |           |                           | messa |
|     |              |           |                           | ge <c |
|     |              |           |                           | ore-r |
|     |              |           |                           | ef-p2 |
|     |              |           |                           | p-net |
|     |              |           |                           | work- |
|     |              |           |                           | maste |
|     |              |           |                           | rnode |
|     |              |           |                           | -mess |
|     |              |           |                           | ages# |
|     |              |           |                           | mnaut |
|     |              |           |                           | h>`__ |
|     |              |           |                           | foll  |
|     |              |           |                           | owing |
|     |              |           |                           | the   |
|     |              |           |                           | `     |
|     |              |           |                           | ``ver |
|     |              |           |                           | ack`` |
|     |              |           |                           | mes   |
|     |              |           |                           | sage  |
|     |              |           |                           | <core |
|     |              |           |                           | -ref- |
|     |              |           |                           | p2p-n |
|     |              |           |                           | etwor |
|     |              |           |                           | k-con |
|     |              |           |                           | trol- |
|     |              |           |                           | messa |
|     |              |           |                           | ges#v |
|     |              |           |                           | erack |
|     |              |           |                           | >`__. |
+-----+--------------+-----------+---------------------------+-------+

The following service identifiers have been assigned.

+-------------+---------------------------+-----------------------------+
| Value       | Name                      | Description                 |
+=============+===========================+=============================+
| 0x00        | *Unnamed*                 | This node is not a full     |
|             |                           | node. It may not be able to |
|             |                           | provide any data except for |
|             |                           | the transactions it         |
|             |                           | originates.                 |
+-------------+---------------------------+-----------------------------+
| 0x01        | ``NODE_NETWORK``          | This is a full node and can |
|             |                           | be asked for full blocks.   |
|             |                           | It should implement all     |
|             |                           | protocol features available |
|             |                           | in its self-reported        |
|             |                           | protocol version.           |
+-------------+---------------------------+-----------------------------+
| 0x02        | ``NODE_GETUTXO``          | This node is capable of     |
|             |                           | responding to the getutxo   |
|             |                           | protocol request. *Dash     |
|             |                           | Core does not support this  |
|             |                           | service.*                   |
+-------------+---------------------------+-----------------------------+
| 0x04        | ``NODE_BLOOM``            | This node is capable and    |
|             |                           | willing to handle           |
|             |                           | bloom-filtered connections. |
|             |                           | Dash Core nodes used to     |
|             |                           | support this by default,    |
|             |                           | without advertising this    |
|             |                           | bit, but no longer do as of |
|             |                           | protocol version 70201 (=   |
|             |                           | NO_BLOOM_VERSION)           |
+-------------+---------------------------+-----------------------------+
| 0x08        | ``NODE_XTHIN``            | This node supports Xtreme   |
|             |                           | Thinblocks. *Dash Core does |
|             |                           | not support this service.*  |
+-------------+---------------------------+-----------------------------+
| 0x400       | ``NODE_NETWORK_LIMITED``  | This is the same as         |
|             |                           | ``NODE_NETWORK`` with the   |
|             |                           | limitation of only serving  |
|             |                           | the last 288 blocks. *Not   |
|             |                           | supported prior to Dash     |
|             |                           | Core 0.16.0*                |
+-------------+---------------------------+-----------------------------+

The following annotated hexdump shows a ```version``
message <core-ref-p2p-network-control-messages#version>`__. (The message
header has been omitted and the actual IP addresses have been replaced
with `RFC5737 <http://tools.ietf.org/html/rfc5737>`__ reserved IP
addresses.)

.. code:: text

   46120100 .................................... Protocol version: 70214
   0500000000000000 ............................ Services: NODE_NETWORK (1) + NODE_BLOOM (4)
   9c10ad5c00000000 ............................ Epoch time: 1554845852

   0100000000000000 ............................ Receiving node's services
   00000000000000000000ffffc61b6409 ............ Receiving node's IPv6 address
   270f ........................................ Receiving node's port number

   0500000000000000 ............................ Transmitting node's services
   00000000000000000000ffffcb0071c0 ............ Transmitting node's IPv6 address
   270f ........................................ Transmitting node's port number

   128035cbc97953f8 ............................ Nonce

   12 .......................................... Bytes in user agent string: 18
   2f4461736820436f72653a302e31322e312e352f..... User agent: /Dash Core:0.14.0/

   851f0b00 .................................... Start height: 76944
   01 .......................................... Relay flag: true

   5dbb5d1baade6a9afa34db708f72c0dd
   b5bd82b3656493484556689640a91357 ............ Masternode Auth. Challenge
