Data Messages
*************

The following network messages all request or provide data related to
transactions and blocks.

.. figure:: https://dash-docs.github.io/img/dev/en-p2p-data-messages.svg
   :alt: Overview Of P2P Protocol Data Request And Reply Messages

   Overview Of P2P Protocol Data Request And Reply Messages

Many of the data messages use <> as unique identifiers for <> and <>.
Inventories have a simple 36-byte structure:

+---------+------------------------+---------------+------------------+
| Bytes   | Name                   | Data Type     | Description      |
+=========+========================+===============+==================+
| 4       | type identifier        | uint32_t      | The type of      |
|         |                        |               | object which was |
|         |                        |               | hashed. See list |
|         |                        |               | of type          |
|         |                        |               | identifiers      |
|         |                        |               | below.           |
+---------+------------------------+---------------+------------------+
| 32      | hash                   | char[32]      | SHA256(SHA256()) |
|         |                        |               | hash of the      |
|         |                        |               | object in        |
|         |                        |               | internal byte    |
|         |                        |               | order.           |
+---------+------------------------+---------------+------------------+

The currently-available type identifiers are:

+----------+--------------------------------------------------+--------+
| Type     | Name                                             | Descr  |
| Id       |                                                  | iption |
| entifier |                                                  |        |
+==========+==================================================+========+
| 1        | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | TXID.  |
+----------+--------------------------------------------------+--------+
| 2        | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is of  |
|          |                                                  | a      |
|          |                                                  | block  |
|          |                                                  | h      |
|          |                                                  | eader. |
+----------+--------------------------------------------------+--------+
| 3        | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is of  |
|          |                                                  | a      |
|          |                                                  | block  |
|          |                                                  | h      |
|          |                                                  | eader; |
|          |                                                  | ide    |
|          |                                                  | ntical |
|          |                                                  | to     |
|          |                                                  | ``     |
|          |                                                  | MSG_BL |
|          |                                                  | OCK``. |
|          |                                                  | When   |
|          |                                                  | used   |
|          |                                                  | in a   |
|          |                                                  | ```get |
|          |                                                  | data`` |
|          |                                                  | me     |
|          |                                                  | ssage  |
|          |                                                  | <core- |
|          |                                                  | ref-p2 |
|          |                                                  | p-netw |
|          |                                                  | ork-da |
|          |                                                  | ta-mes |
|          |                                                  | sages# |
|          |                                                  | getdat |
|          |                                                  | a>`__, |
|          |                                                  | this   |
|          |                                                  | ind    |
|          |                                                  | icates |
|          |                                                  | the    |
|          |                                                  | re     |
|          |                                                  | sponse |
|          |                                                  | should |
|          |                                                  | be a   |
|          |                                                  | ```m   |
|          |                                                  | erkleb |
|          |                                                  | lock`` |
|          |                                                  | messa  |
|          |                                                  | ge <co |
|          |                                                  | re-ref |
|          |                                                  | -p2p-n |
|          |                                                  | etwork |
|          |                                                  | -data- |
|          |                                                  | messag |
|          |                                                  | es#mer |
|          |                                                  | kleblo |
|          |                                                  | ck>`__ |
|          |                                                  | rather |
|          |                                                  | than a |
|          |                                                  | ```b   |
|          |                                                  | lock`` |
|          |                                                  | messa  |
|          |                                                  | ge <co |
|          |                                                  | re-ref |
|          |                                                  | -p2p-n |
|          |                                                  | etwork |
|          |                                                  | -data- |
|          |                                                  | messag |
|          |                                                  | es#blo |
|          |                                                  | ck>`__ |
|          |                                                  | (but   |
|          |                                                  | this   |
|          |                                                  | only   |
|          |                                                  | works  |
|          |                                                  | if a   |
|          |                                                  | bloom  |
|          |                                                  | filter |
|          |                                                  | was    |
|          |                                                  | prev   |
|          |                                                  | iously |
|          |                                                  | config |
|          |                                                  | ured). |
|          |                                                  | **Only |
|          |                                                  | for    |
|          |                                                  | use    |
|          |                                                  | in**\  |
|          |                                                  | ```get |
|          |                                                  | data`` |
|          |                                                  | mes    |
|          |                                                  | sages  |
|          |                                                  | <core- |
|          |                                                  | ref-p2 |
|          |                                                  | p-netw |
|          |                                                  | ork-da |
|          |                                                  | ta-mes |
|          |                                                  | sages# |
|          |                                                  | getdat |
|          |                                                  | a>`__\ |
|          |                                                  |  **.** |
+----------+--------------------------------------------------+--------+
| 4        | <>                                               | ``MS   |
|          |                                                  | G_TXLO |
|          |                                                  | CK_REQ |
|          |                                                  | UEST`` |
|          |                                                  | prior  |
|          |                                                  | to     |
|          |                                                  | Dash   |
|          |                                                  | Core   |
|          |                                                  | 0      |
|          |                                                  | .15.0. |
|          |                                                  | The    |
|          |                                                  | hash   |
|          |                                                  | is an  |
|          |                                                  | I      |
|          |                                                  | nstant |
|          |                                                  | Send   |
|          |                                                  | trans  |
|          |                                                  | action |
|          |                                                  | lock   |
|          |                                                  | re     |
|          |                                                  | quest. |
|          |                                                  | Transa |
|          |                                                  | ctions |
|          |                                                  | re     |
|          |                                                  | ceived |
|          |                                                  | this   |
|          |                                                  | way    |
|          |                                                  | are    |
|          |                                                  | a      |
|          |                                                  | utomat |
|          |                                                  | ically |
|          |                                                  | con    |
|          |                                                  | verted |
|          |                                                  | to a   |
|          |                                                  | st     |
|          |                                                  | andard |
|          |                                                  | `      |
|          |                                                  | ``tx`` |
|          |                                                  | me     |
|          |                                                  | ssage  |
|          |                                                  | <core- |
|          |                                                  | ref-p2 |
|          |                                                  | p-netw |
|          |                                                  | ork-da |
|          |                                                  | ta-mes |
|          |                                                  | sages# |
|          |                                                  | tx>`__ |
|          |                                                  | as of  |
|          |                                                  | Dash   |
|          |                                                  | Core   |
|          |                                                  | 0      |
|          |                                                  | .15.0. |
+----------+--------------------------------------------------+--------+
| 6        | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is     |
|          |                                                  | Spork  |
|          |                                                  | ID.    |
+----------+--------------------------------------------------+--------+
| 16       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is     |
|          |                                                  | Co     |
|          |                                                  | inJoin |
|          |                                                  | Bro    |
|          |                                                  | adcast |
|          |                                                  | TX.    |
+----------+--------------------------------------------------+--------+
| 17       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | Gove   |
|          |                                                  | rnance |
|          |                                                  | O      |
|          |                                                  | bject. |
+----------+--------------------------------------------------+--------+
| 18       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | Gove   |
|          |                                                  | rnance |
|          |                                                  | Object |
|          |                                                  | Vote.  |
+----------+--------------------------------------------------+--------+
| 20       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is of  |
|          |                                                  | a      |
|          |                                                  | block  |
|          |                                                  | h      |
|          |                                                  | eader; |
|          |                                                  | ide    |
|          |                                                  | ntical |
|          |                                                  | to     |
|          |                                                  | ``     |
|          |                                                  | MSG_BL |
|          |                                                  | OCK``. |
|          |                                                  | When   |
|          |                                                  | used   |
|          |                                                  | in a   |
|          |                                                  | ```get |
|          |                                                  | data`` |
|          |                                                  | me     |
|          |                                                  | ssage  |
|          |                                                  | <core- |
|          |                                                  | ref-p2 |
|          |                                                  | p-netw |
|          |                                                  | ork-da |
|          |                                                  | ta-mes |
|          |                                                  | sages# |
|          |                                                  | getdat |
|          |                                                  | a>`__, |
|          |                                                  | this   |
|          |                                                  | ind    |
|          |                                                  | icates |
|          |                                                  | the    |
|          |                                                  | re     |
|          |                                                  | sponse |
|          |                                                  | should |
|          |                                                  | be a   |
|          |                                                  | ```    |
|          |                                                  | cmpctb |
|          |                                                  | lock`` |
|          |                                                  | messa  |
|          |                                                  | ge <co |
|          |                                                  | re-ref |
|          |                                                  | -p2p-n |
|          |                                                  | etwork |
|          |                                                  | -data- |
|          |                                                  | messag |
|          |                                                  | es#cmp |
|          |                                                  | ctbloc |
|          |                                                  | k>`__. |
|          |                                                  | **Only |
|          |                                                  | for    |
|          |                                                  | use    |
|          |                                                  | in**\  |
|          |                                                  | ```get |
|          |                                                  | data`` |
|          |                                                  | mes    |
|          |                                                  | sages  |
|          |                                                  | <core- |
|          |                                                  | ref-p2 |
|          |                                                  | p-netw |
|          |                                                  | ork-da |
|          |                                                  | ta-mes |
|          |                                                  | sages# |
|          |                                                  | getdat |
|          |                                                  | a>`__\ |
|          |                                                  |  **.** |
+----------+--------------------------------------------------+--------+
| 21       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | long-  |
|          |                                                  | living |
|          |                                                  | mast   |
|          |                                                  | ernode |
|          |                                                  | quorum |
|          |                                                  | final  |
|          |                                                  | c      |
|          |                                                  | ommitm |
|          |                                                  | ent.\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 0      |
|          |                                                  | .13.0* |
+----------+--------------------------------------------------+--------+
| 23       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | long-  |
|          |                                                  | living |
|          |                                                  | mast   |
|          |                                                  | ernode |
|          |                                                  | quorum |
|          |                                                  | con    |
|          |                                                  | tribut |
|          |                                                  | ion.\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 0      |
|          |                                                  | .14.0* |
+----------+--------------------------------------------------+--------+
| 24       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | long-  |
|          |                                                  | living |
|          |                                                  | mast   |
|          |                                                  | ernode |
|          |                                                  | quorum |
|          |                                                  | compla |
|          |                                                  | int.\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 0      |
|          |                                                  | .14.0* |
+----------+--------------------------------------------------+--------+
| 25       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | long-  |
|          |                                                  | living |
|          |                                                  | mast   |
|          |                                                  | ernode |
|          |                                                  | quorum |
|          |                                                  | just   |
|          |                                                  | ificat |
|          |                                                  | ion.\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 0      |
|          |                                                  | .14.0* |
+----------+--------------------------------------------------+--------+
| 26       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | long-  |
|          |                                                  | living |
|          |                                                  | mast   |
|          |                                                  | ernode |
|          |                                                  | quorum |
|          |                                                  | pre    |
|          |                                                  | mature |
|          |                                                  | c      |
|          |                                                  | ommitm |
|          |                                                  | ent.\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 0      |
|          |                                                  | .14.0* |
+----------+--------------------------------------------------+--------+
| 28       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | long-  |
|          |                                                  | living |
|          |                                                  | mast   |
|          |                                                  | ernode |
|          |                                                  | quorum |
|          |                                                  | rec    |
|          |                                                  | overed |
|          |                                                  | sign   |
|          |                                                  | ature. |
|          |                                                  | \ **N  |
|          |                                                  | ote**: |
|          |                                                  | Only   |
|          |                                                  | r      |
|          |                                                  | elayed |
|          |                                                  | to     |
|          |                                                  | other  |
|          |                                                  | maste  |
|          |                                                  | rnodes |
|          |                                                  | in the |
|          |                                                  | same   |
|          |                                                  | quorum |
|          |                                                  | and    |
|          |                                                  | nodes  |
|          |                                                  | that   |
|          |                                                  | have   |
|          |                                                  | sent a |
|          |                                                  | ```qw  |
|          |                                                  | atch`` |
|          |                                                  | me     |
|          |                                                  | ssage  |
|          |                                                  | <core- |
|          |                                                  | ref-p2 |
|          |                                                  | p-netw |
|          |                                                  | ork-qu |
|          |                                                  | orum-m |
|          |                                                  | essage |
|          |                                                  | s#qwat |
|          |                                                  | ch>`__ |
|          |                                                  | as of  |
|          |                                                  | Dash   |
|          |                                                  | Core   |
|          |                                                  | 0.     |
|          |                                                  | 17.0\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 0      |
|          |                                                  | .14.0* |
+----------+--------------------------------------------------+--------+
| 29       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is a   |
|          |                                                  | Cha    |
|          |                                                  | inLock |
|          |                                                  | signat |
|          |                                                  | ure.\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 0      |
|          |                                                  | .14.0* |
+----------+--------------------------------------------------+--------+
| 30       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is an  |
|          |                                                  | LLMQ   |
|          |                                                  | -based |
|          |                                                  | Insta  |
|          |                                                  | ntSend |
|          |                                                  | lock   |
|          |                                                  | (`DIP1 |
|          |                                                  | 0 <htt |
|          |                                                  | ps://g |
|          |                                                  | ithub. |
|          |                                                  | com/da |
|          |                                                  | shpay/ |
|          |                                                  | dips/b |
|          |                                                  | lob/ma |
|          |                                                  | ster/d |
|          |                                                  | ip-001 |
|          |                                                  | 0.md>` |
|          |                                                  | __).\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 0      |
|          |                                                  | .14.0* |
+----------+--------------------------------------------------+--------+
| 31       | <>                                               | The    |
|          |                                                  | hash   |
|          |                                                  | is an  |
|          |                                                  | LLMQ   |
|          |                                                  | -based |
|          |                                                  | d      |
|          |                                                  | etermi |
|          |                                                  | nistic |
|          |                                                  | Insta  |
|          |                                                  | ntSend |
|          |                                                  | lock   |
|          |                                                  | (`DIP2 |
|          |                                                  | 2 <htt |
|          |                                                  | ps://g |
|          |                                                  | ithub. |
|          |                                                  | com/da |
|          |                                                  | shpay/ |
|          |                                                  | dips/b |
|          |                                                  | lob/ma |
|          |                                                  | ster/d |
|          |                                                  | ip-002 |
|          |                                                  | 2.md>` |
|          |                                                  | __).\  |
|          |                                                  | *Added |
|          |                                                  | in     |
|          |                                                  | 18.0*  |
+----------+--------------------------------------------------+--------+

**Deprecated Type Identifiers**

The following type identifiers have been deprecated recently. To see
type identifiers removed longer ago, please see the `previous version of
documentation <https://dashcore.readme.io/v0.16.0/docs/core-ref-p2p-network-data-messages>`__.

+-----------------+------+-------------------------------------------+
| Type Identifier | Name | Description                               |
+=================+======+===========================================+
| 5               | <>   | **Deprecated in 0.15.0**\ The hash is an  |
|                 |      | Instant Send transaction vote.            |
+-----------------+------+-------------------------------------------+

Type identifier zero and type identifiers greater than those shown in
the table above are reserved for future implementations. Dash Core
ignores all inventories with one of these unknown types.

block
=====

The ```block`` message <core-ref-p2p-network-data-messages#block>`__
transmits a single <> in the format described in the `serialized blocks
section <core-ref-block-chain-serialized-blocks>`__. See that section
for an example hexdump. It can be sent for two different reasons:

1. **GetData Response:** Nodes will always send it in response to a
   ```getdata`` message <core-ref-p2p-network-data-messages#getdata>`__
   that requests the block with an <> type of ``MSG_BLOCK`` (provided
   the node has that block available for relay).

2. **Unsolicited:** Some miners will send unsolicited ```block``
   messages <core-ref-p2p-network-data-messages#block>`__ broadcasting
   their newly-mined blocks to all of their <>. Many <> pools do the
   same thing, although some may be misconfigured to send the block from
   multiple nodes, possibly sending the same block to some peers more
   than once.

blocktxn
========

*Added in protocol version 70209 of Dash Core as described by BIP152*

The ```blocktxn``
message <core-ref-p2p-network-data-messages#blocktxn>`__ sends requested
<> <> to a node which previously requested them with a ```getblocktxn``
message <core-ref-p2p-network-data-messages#getblocktxn>`__. It is
defined as a message containing a serialized ``BlockTransactions``
message.

Upon receipt of a properly-formatted requested ```blocktxn``
message <core-ref-p2p-network-data-messages#blocktxn>`__, <> should:

1. Attempt to reconstruct the full block by taking the prefilledtxn
   transactions from the original ```cmpctblock``
   message <core-ref-p2p-network-data-messages#cmpctblock>`__ and
   placing them in the marked positions
2. For each short transaction ID from the original ```cmpctblock``
   message <core-ref-p2p-network-data-messages#cmpctblock>`__, in order,
   find the corresponding transaction (from either the ```blocktxn``
   message <core-ref-p2p-network-data-messages#blocktxn>`__ or from
   other sources)
3. Place each short transaction ID in the first available position in
   the block
4. Once the block has been reconstructed, it shall be processed as
   normal.

**Short transaction IDs are expected to occasionally collide. Nodes must
not be penalized for such collisions.**

The structure of ``BlockTransactions`` is defined below.

+--------+-------------------+-------------------+--------+----------+
| Bytes  | Name              | Data Type         | En     | Des      |
|        |                   |                   | coding | cription |
+========+===================+===================+========+==========+
| 32     | blockhash         | Binary blob       | The    | The      |
|        |                   |                   | output | b        |
|        |                   |                   | from a | lockhash |
|        |                   |                   | d      | of the   |
|        |                   |                   | ouble- | block    |
|        |                   |                   | SHA256 | which    |
|        |                   |                   | of the | the      |
|        |                   |                   | block  | tran     |
|        |                   |                   | h      | sactions |
|        |                   |                   | eader, | being    |
|        |                   |                   | as     | provided |
|        |                   |                   | used   | are in   |
|        |                   |                   | els    |          |
|        |                   |                   | ewhere |          |
+--------+-------------------+-------------------+--------+----------+
| 1 or 3 | tra               | CompactSize       | As     | The      |
|        | nsactions\_length |                   | used   | number   |
|        |                   |                   | to     | of       |
|        |                   |                   | encode | tran     |
|        |                   |                   | array  | sactions |
|        |                   |                   | l      | provided |
|        |                   |                   | engths |          |
|        |                   |                   | els    |          |
|        |                   |                   | ewhere |          |
+--------+-------------------+-------------------+--------+----------+
| *V     | transactions      | List of           | As     | The      |
| aries* |                   | transactions      | e      | tran     |
|        |                   |                   | ncoded | sactions |
|        |                   |                   | in     | provided |
|        |                   |                   | `      |          |
|        |                   |                   | ``tx`` |          |
|        |                   |                   | mes    |          |
|        |                   |                   | sages  |          |
|        |                   |                   | <core- |          |
|        |                   |                   | ref-p2 |          |
|        |                   |                   | p-netw |          |
|        |                   |                   | ork-da |          |
|        |                   |                   | ta-mes |          |
|        |                   |                   | sages# |          |
|        |                   |                   | tx>`__ |          |
|        |                   |                   | in     |          |
|        |                   |                   | re     |          |
|        |                   |                   | sponse |          |
|        |                   |                   | to     |          |
|        |                   |                   | ``getd |          |
|        |                   |                   | ata MS |          |
|        |                   |                   | G_TX`` |          |
+--------+-------------------+-------------------+--------+----------+

The following annotated hexdump shows a ```blocktxn``
message <core-ref-p2p-network-data-messages#blocktxn>`__. (The message
header has been omitted.)

.. code:: text

   182327cb727da7d60541da793831fd0ab0509e79c8cd
   3d654cdf3a0100000000 ....................... Block Hash

   01 ......................................... Transactions Provided: 1

   Transaction(s)
   | Transaction 1
   | | 01000000 ................................ Transaction Version: 1
   | | 01 ...................................... Input count: 1
   | |
   | | Transaction input #1
   | | |
   | | | 0952617a516d956e2ecee71a6adc249f
   | | | 4bb757adcc409452ab98c8e55c31e62a ..... Outpoint TXID
   | | | 00000000 ............................. Outpoint index number: 0
   | | |
   | | | 6b ................................... Bytes in sig. script: 107
   | | | 483045022100d10edf447252e1e69ff1
   | | | 77330bb2c889a50be02e00cc5d79c0d0
   | | | 79ae56518fc40220245d36905dc950fc
   | | | d55694cfde8cde3109dc80b12aca3a6e
   | | | 332033802ee36e1b01210272cc6e7660
   | | | 2648831d8e80fca8eb24369cd0f23ff0
   | | | 79cf20ae9d9beee05de6db ............... Secp256k1 signature
   | | |
   | | | ffffffff ............................. Sequence number: UINT32_MAX
   | |
   | | 02 ..................................... Number of outputs: 02
   | |
   | | Transaction output #1
   | | | 0be0f50500000000 ..................... Duffs (0.99999755 Dash)
   | | |
   | | | 19 ................................... Bytes in pubkey script: 25
   | | | | 76 ................................. OP_DUP
   | | | | a9 ................................. OP_HASH160
   | | | | 14 ................................. Push 20 bytes as data
   | | | | | 923d91ed359f650eec6ea8b9030b340d
   | | | | | ea63d590 ......................... PubKey hash
   | | | | 88 ................................. OP_EQUALVERIFY
   | | | | ac ................................. OP_CHECKSIG
   | |
   | | [...] .................................. 1 more tx output omitted
   | |
   | | 00000000 ............................... locktime: 0 (a block height)

cfcheckpt
=========

*Added in protocol version 70223 of Dash Core as described by*\ `BIP
157 <https://github.com/bitcoin/bips/blob/master/bip-0157.mediawiki>`__\ *.*

The ```cfcheckpt``
message <core-ref-p2p-network-data-messages#cfcheckpt>`__ is sent in
response to the ```getcfcheckpt``
message <core-ref-p2p-network-data-messages#getcfcheckpt>`__. The filter
headers included are the set of all filter headers on the requested
chain where the height is a positive multiple of 1,000.

+---------+-----------------------+------------------+----------------+
| Bytes   | Name                  | Data Type        | Description    |
+=========+=======================+==================+================+
| 1       | filter_type           | uint8_t          | Filter type    |
|         |                       |                  | for which      |
|         |                       |                  | headers are    |
|         |                       |                  | requested.     |
|         |                       |                  | Should match   |
|         |                       |                  | the field in   |
|         |                       |                  | the            |
|         |                       |                  | ```            |
|         |                       |                  | getcfcheckpt`` |
|         |                       |                  | mess           |
|         |                       |                  | age <core-ref- |
|         |                       |                  | p2p-network-da |
|         |                       |                  | ta-messages#ge |
|         |                       |                  | tcfcheckpt>`__ |
|         |                       |                  | being          |
|         |                       |                  | responded to.  |
+---------+-----------------------+------------------+----------------+
| 32      | stop_hash             | uint256          | The hash of    |
|         |                       |                  | the last block |
|         |                       |                  | in the         |
|         |                       |                  | requested      |
|         |                       |                  | range. Should  |
|         |                       |                  | match the      |
|         |                       |                  | field in the   |
|         |                       |                  | ```            |
|         |                       |                  | getcfcheckpt`` |
|         |                       |                  | mess           |
|         |                       |                  | age <core-ref- |
|         |                       |                  | p2p-network-da |
|         |                       |                  | ta-messages#ge |
|         |                       |                  | tcfcheckpt>`__ |
|         |                       |                  | being          |
|         |                       |                  | responded to.  |
+---------+-----------------------+------------------+----------------+
| 1-3     | f                     | CompactSize      | The length of  |
|         | ilter_headers\_length |                  | the following  |
|         |                       |                  | vector of      |
|         |                       |                  | filter headers |
+---------+-----------------------+------------------+----------------+
| ``fil   | filter_hashes         | [][32]byte       | The filter     |
| ter_hea |                       |                  | headers at     |
| ders``\ |                       |                  | intervals of   |
|  \ ``_l |                       |                  | 1,000          |
| ength`` |                       |                  |                |
| \* 32   |                       |                  |                |
+---------+-----------------------+------------------+----------------+

cfheaders
=========

*Added in protocol version 70223 of Dash Core as described by*\ `BIP
157 <https://github.com/bitcoin/bips/blob/master/bip-0157.mediawiki>`__\ *.*

The ```cfheaders``
message <core-ref-p2p-network-data-messages#cfheaders>`__ is sent in
response to the ```getcfheaders``
message <core-ref-p2p-network-data-messages#getcfheaders>`__. Instead of
including the filter headers themselves, the response includes one
filter header and a sequence of filter hashes, from which the headers
can be derived. This has the benefit that the client can verify the
binding links between the headers.

+---------+-----------------------+------------------+----------------+
| Bytes   | Name                  | Data Type        | Description    |
+=========+=======================+==================+================+
| 1       | filter_type           | uint8_t          | Filter type    |
|         |                       |                  | for which      |
|         |                       |                  | headers are    |
|         |                       |                  | requested.     |
|         |                       |                  | Should match   |
|         |                       |                  | the field in   |
|         |                       |                  | the            |
|         |                       |                  | ```            |
|         |                       |                  | getcfheaders`` |
|         |                       |                  | mess           |
|         |                       |                  | age <core-ref- |
|         |                       |                  | p2p-network-da |
|         |                       |                  | ta-messages#ge |
|         |                       |                  | tcfheaders>`__ |
|         |                       |                  | being          |
|         |                       |                  | responded to.  |
+---------+-----------------------+------------------+----------------+
| 32      | stop_hash             | uint256          | The hash of    |
|         |                       |                  | the last block |
|         |                       |                  | in the         |
|         |                       |                  | requested      |
|         |                       |                  | range. Should  |
|         |                       |                  | match the      |
|         |                       |                  | field in the   |
|         |                       |                  | ```            |
|         |                       |                  | getcfheaders`` |
|         |                       |                  | mess           |
|         |                       |                  | age <core-ref- |
|         |                       |                  | p2p-network-da |
|         |                       |                  | ta-messages#ge |
|         |                       |                  | tcfheaders>`__ |
|         |                       |                  | being          |
|         |                       |                  | responded to.  |
+---------+-----------------------+------------------+----------------+
| 32      | pr                    | uint256          | The filter     |
|         | evious_filter\_header |                  | header         |
|         |                       |                  | preceding the  |
|         |                       |                  | first block in |
|         |                       |                  | the requested  |
|         |                       |                  | range          |
+---------+-----------------------+------------------+----------------+
| 1-3     | filter_hashes\_length | CompactSize      | The length of  |
|         |                       |                  | the following  |
|         |                       |                  | vector of      |
|         |                       |                  | filter hashes. |
|         |                       |                  | Must not be >  |
|         |                       |                  | 2000.          |
+---------+-----------------------+------------------+----------------+
| ``fi    | filter_hashes         | [][32]byte       | The filter     |
| lter_ha |                       |                  | hashes for     |
| shes``\ |                       |                  | each block in  |
|  \ ``_l |                       |                  | the requested  |
| ength`` |                       |                  | range          |
| \* 32   |                       |                  |                |
+---------+-----------------------+------------------+----------------+

cfilter
=======

*Added in protocol version 70223 of Dash Core as described by*\ `BIP
157 <https://github.com/bitcoin/bips/blob/master/bip-0157.mediawiki>`__\ *.*

The ```cfilter`` message <core-ref-p2p-network-data-messages#cfilter>`__
is sent in response to the ```getcfilters``
message <core-ref-p2p-network-data-messages#getcfilters>`__, one for
each block in the requested range.

+---------+-----------------------+------------------+----------------+
| Bytes   | Name                  | Data Type        | Description    |
+=========+=======================+==================+================+
| 1       | filter_type           | uint8_t          | Filter type    |
|         |                       |                  | for which      |
|         |                       |                  | headers are    |
|         |                       |                  | requested.     |
|         |                       |                  | Should match   |
|         |                       |                  | the field in   |
|         |                       |                  | the            |
|         |                       |                  | ``             |
|         |                       |                  | `getcfilters`` |
|         |                       |                  | mes            |
|         |                       |                  | sage <core-ref |
|         |                       |                  | -p2p-network-d |
|         |                       |                  | ata-messages#g |
|         |                       |                  | etcfilters>`__ |
|         |                       |                  | being          |
|         |                       |                  | responded to.  |
+---------+-----------------------+------------------+----------------+
| 32      | block_hash            | uint256          | Block hash of  |
|         |                       |                  | the block for  |
|         |                       |                  | which the      |
|         |                       |                  | filter is      |
|         |                       |                  | being          |
|         |                       |                  | returned. Must |
|         |                       |                  | correspond to  |
|         |                       |                  | a block that   |
|         |                       |                  | is an ancestor |
|         |                       |                  | of the         |
|         |                       |                  | request’s      |
|         |                       |                  | ``stop_hash``  |
|         |                       |                  | with a height  |
|         |                       |                  | >=             |
|         |                       |                  | ``s            |
|         |                       |                  | tart_height``. |
+---------+-----------------------+------------------+----------------+
| 1-5     | num_filter_bytes      | CompactSize      | A variable     |
|         |                       |                  | length integer |
|         |                       |                  | representing   |
|         |                       |                  | the size of    |
|         |                       |                  | the filter in  |
|         |                       |                  | the following  |
|         |                       |                  | field          |
+---------+-----------------------+------------------+----------------+
| ``num_  | filter_bytes          | []bytes          | The serialized |
| filter_ |                       |                  | compact filter |
| bytes`` |                       |                  | for this block |
+---------+-----------------------+------------------+----------------+

cmpctblock
==========

*Added in protocol version 70209 of Dash Core as described by BIP152*

The ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__ is a reply to
a ```getdata`` message <core-ref-p2p-network-data-messages#getdata>`__
which requested a <> using the <> type ``MSG_CMPCT_BLOCK``. If the
requested block was recently announced and is close to the tip of the
best chain of the receiver and after having sent the requesting <> a
```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__, nodes
respond with a ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__ containing
data for the block.

**If the requested block is too old, the node responds with a full
non-compact block**

Upon receipt of a ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__, after
sending a ```sendcmpct``
message <core-ref-p2p-network-control-messages#sendcmpct>`__, nodes
should calculate the short transaction ID for each <> they have
available (i.e. in their mempool) and compare each to each short
transaction ID in the ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__. After
finding already-available transactions, nodes which do not have all
transactions available to reconstruct the full block should request the
missing transactions using a ```getblocktxn``
message <core-ref-p2p-network-data-messages#getblocktxn>`__.

A node must not send a ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__ unless they
are able to respond to a ```getblocktxn``
message <core-ref-p2p-network-data-messages#getblocktxn>`__ which
requests every transaction in the block. A node must not send a
```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__ without
having validated that the <> properly commits to each transaction in the
block, and properly builds on top of the existing, fully-validated chain
with a valid proof-of-work either as a part of the current most-work
valid chain, or building directly on top of it. A node may send a
```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__ before
validating that each transaction in the block validly spends existing
UTXO set entries.

The ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__ contains a
vector of ``PrefilledTransaction`` whose structure is defined below. A
``PrefilledTransaction`` is used in ``HeaderAndShortIDs`` to provide a
list of a few transactions explicitly.

+--------+-------------------+-------------------+--------+----------+
| Bytes  | Name              | Data Type         | En     | Des      |
|        |                   |                   | coding | cription |
+========+===================+===================+========+==========+
| 1 or 3 | index             | CompactSize       | C      | The      |
|        |                   |                   | ompact | index    |
|        |                   |                   | Size,  | into the |
|        |                   |                   | di     | block at |
|        |                   |                   | fferen | which    |
|        |                   |                   | tially | this     |
|        |                   |                   | e      | tra      |
|        |                   |                   | ncoded | nsaction |
|        |                   |                   | since  | is       |
|        |                   |                   | the    |          |
|        |                   |                   | last   |          |
|        |                   |                   | Pr     |          |
|        |                   |                   | efille |          |
|        |                   |                   | dTrans |          |
|        |                   |                   | action |          |
|        |                   |                   | in a   |          |
|        |                   |                   | list   |          |
+--------+-------------------+-------------------+--------+----------+
| *V     | tx                | Transaction       | As     | Tra      |
| aries* |                   |                   | e      | nsaction |
|        |                   |                   | ncoded | which is |
|        |                   |                   | in     | in the   |
|        |                   |                   | `      | block at |
|        |                   |                   | ``tx`` | index    |
|        |                   |                   | mes    | `        |
|        |                   |                   | sages  | `index`` |
|        |                   |                   | <core- |          |
|        |                   |                   | ref-p2 |          |
|        |                   |                   | p-netw |          |
|        |                   |                   | ork-da |          |
|        |                   |                   | ta-mes |          |
|        |                   |                   | sages# |          |
|        |                   |                   | tx>`__ |          |
|        |                   |                   | sent   |          |
|        |                   |                   | in     |          |
|        |                   |                   | re     |          |
|        |                   |                   | sponse |          |
|        |                   |                   | to     |          |
|        |                   |                   | ``getd |          |
|        |                   |                   | ata MS |          |
|        |                   |                   | G_TX`` |          |
+--------+-------------------+-------------------+--------+----------+

The ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__ is
compromised of a serialized ``HeaderAndShortIDs`` structure which is
defined below. A ``HeaderAndShortIDs`` structure is used to relay a
block header, the short transactions IDs used for matching
already-available transactions, and a select few transactions which we
expect a peer may be missing.

+--------+-------------------+-------------------+--------+----------+
| Bytes  | Name              | Data Type         | En     | Des      |
|        |                   |                   | coding | cription |
+========+===================+===================+========+==========+
| 80     | header            | Block header      | First  | The      |
|        |                   |                   | 80     | header   |
|        |                   |                   | bytes  | of the   |
|        |                   |                   | of the | block    |
|        |                   |                   | block  | being    |
|        |                   |                   | as     | provided |
|        |                   |                   | d      |          |
|        |                   |                   | efined |          |
|        |                   |                   | by the |          |
|        |                   |                   | en     |          |
|        |                   |                   | coding |          |
|        |                   |                   | used   |          |
|        |                   |                   | by     |          |
|        |                   |                   | ```b   |          |
|        |                   |                   | lock`` |          |
|        |                   |                   | messag |          |
|        |                   |                   | es <co |          |
|        |                   |                   | re-ref |          |
|        |                   |                   | -p2p-n |          |
|        |                   |                   | etwork |          |
|        |                   |                   | -data- |          |
|        |                   |                   | messag |          |
|        |                   |                   | es#blo |          |
|        |                   |                   | ck>`__ |          |
+--------+-------------------+-------------------+--------+----------+
| 8      | nonce             | uint64_t          | Little | A nonce  |
|        |                   |                   | Endian | for use  |
|        |                   |                   |        | in short |
|        |                   |                   |        | tra      |
|        |                   |                   |        | nsaction |
|        |                   |                   |        | ID       |
|        |                   |                   |        | calc     |
|        |                   |                   |        | ulations |
+--------+-------------------+-------------------+--------+----------+
| 1 or 3 | shortids\_length  | CompactSize       | As     | The      |
|        |                   |                   | used   | number   |
|        |                   |                   | to     | of short |
|        |                   |                   | encode | tra      |
|        |                   |                   | array  | nsaction |
|        |                   |                   | l      | IDs in   |
|        |                   |                   | engths | ``sh     |
|        |                   |                   | els    | ortids`` |
|        |                   |                   | ewhere | (i.      |
|        |                   |                   |        | e. block |
|        |                   |                   |        | tx count |
|        |                   |                   |        | -        |
|        |                   |                   |        | ``prefil |
|        |                   |                   |        | ledtxn`` |
|        |                   |                   |        | \ \ ``_l |
|        |                   |                   |        | ength``) |
+--------+-------------------+-------------------+--------+----------+
| *V     | shortids          | List of 6-byte    | Little | The      |
| aries* |                   | integers          | Endian | short    |
|        |                   |                   |        | tra      |
|        |                   |                   |        | nsaction |
|        |                   |                   |        | IDs      |
|        |                   |                   |        | ca       |
|        |                   |                   |        | lculated |
|        |                   |                   |        | from the |
|        |                   |                   |        | tran     |
|        |                   |                   |        | sactions |
|        |                   |                   |        | which    |
|        |                   |                   |        | were not |
|        |                   |                   |        | provided |
|        |                   |                   |        | ex       |
|        |                   |                   |        | plicitly |
|        |                   |                   |        | in       |
|        |                   |                   |        | ``prefil |
|        |                   |                   |        | ledtxn`` |
+--------+-------------------+-------------------+--------+----------+
| 1 or 3 | pre               | CompactSize       | As     | The      |
|        | filledtxn\_length |                   | used   | number   |
|        |                   |                   | to     | of       |
|        |                   |                   | encode | p        |
|        |                   |                   | array  | refilled |
|        |                   |                   | l      | tran     |
|        |                   |                   | engths | sactions |
|        |                   |                   | els    | in       |
|        |                   |                   | ewhere | ``prefil |
|        |                   |                   |        | ledtxn`` |
|        |                   |                   |        | (i.      |
|        |                   |                   |        | e. block |
|        |                   |                   |        | tx count |
|        |                   |                   |        | -        |
|        |                   |                   |        | ``sh     |
|        |                   |                   |        | ortids`` |
|        |                   |                   |        | \ \ ``_l |
|        |                   |                   |        | ength``) |
+--------+-------------------+-------------------+--------+----------+
| *V     | prefilledtxn      | List of           | As     | Used to  |
| aries* |                   | Pref              | d      | provide  |
|        |                   | illedTransactions | efined | the      |
|        |                   |                   | by     | coinbase |
|        |                   |                   | ``     | tra      |
|        |                   |                   | Prefil | nsaction |
|        |                   |                   | led``\ | and a    |
|        |                   |                   |  \ ``T | select   |
|        |                   |                   | ransac | few      |
|        |                   |                   | tion`` | which we |
|        |                   |                   | defi   | expect a |
|        |                   |                   | nition | peer may |
|        |                   |                   | below  | be       |
|        |                   |                   |        | missing  |
+--------+-------------------+-------------------+--------+----------+

**Short Transaction ID calculation**
------------------------------------

Short transaction IDs are used to represent a transaction without
sending a full 256-bit hash. They are calculated as follows,

-  A single-SHA256 hashing the <> with the nonce appended (in
   little-endian)
-  Running SipHash-2-4 with the input being the transaction ID and the
   keys (k0/k1) set to the first two little-endian 64-bit integers from
   the above hash, respectively.
-  Dropping the 2 most significant bytes from the SipHash output to make
   it 6 bytes.

The following annotated hexdump shows a ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__. (The message
header has been omitted.)

.. code:: text

   00000020981178a4342cec6316296b2ad84c9b7cdf9f
   2688e5d0fe1a0003cd0000000000f64870f52a3d0125
   1336c9464961216732b25fbf288a51f25a0e81bffb20
   e9600194d85a64a50d1cc02b0181 ................ Block Header

   3151b67e5b418b9d ............................ Nonce

   01 .......................................... Short IDs Length: 1
   483edcd3c799 ................................ Short IDs

   01 .......................................... Prefilled Transaction Length: 1

   Prefilled Transactions
   | 00 ........................................ Index: 0
   |
   | Transaction 1 (Coinbase)
   | | 01000000 ................................ Transaction Version: 1
   | | 01 ...................................... Input count: 1
   | |
   | | Transaction input #1
   | | |
   | | | 00000000000000000000000000000000
   | | | 00000000000000000000000000000000 ..... Outpoint TXID
   | | | ffffffff ............................. Outpoint index number: UINT32_MAX
   | | |
   | | | 13 ................................... Bytes in sig. script: 19
   | | | 03daaf010e2f5032506f6f6c2d74444153482f Secp256k1 signature
   | | |
   | | | ffffffff ............................. Sequence number: UINT32_MAX
   | |
   | | 04 ..................................... Number of outputs: 04
   | |
   | | Transaction output #1
   | | | ffe5654200000000 ..................... Duffs (11.13974271 Dash)
   | | |
   | | | 19 ................................... Bytes in pubkey script: 25
   | | | | 76 ................................. OP_DUP
   | | | | a9 ................................. OP_HASH160
   | | | | 14 ................................. Push 20 bytes as data
   | | | | | b885cb21ad12e593c1a46d814df47ccb
   | | | | | 450a7d84 ......................... PubKey hash
   | | | | 88 ................................. OP_EQUALVERIFY
   | | | | ac ................................. OP_CHECKSIG
   | |
   | | [...] .................................. 3 more tx outputs omitted
   | |
   | | 00000000 ............................... locktime: 0 (a block height)

getblocks
=========

The ```getblocks``
message <core-ref-p2p-network-data-messages#getblocks>`__ requests an
```inv`` message <core-ref-p2p-network-data-messages#inv>`__ that
provides <> hashes starting from a particular point in the <>. It allows
a <> which has been disconnected or started for the first time to get
the data it needs to request the blocks it hasn’t seen.

Peers which have been disconnected may have <> in their locally-stored
block chain, so the ```getblocks``
message <core-ref-p2p-network-data-messages#getblocks>`__ allows the
requesting peer to provide the receiving peer with multiple <> hashes at
heights on their local chain. This allows the receiving peer to find,
within that list, the last header hash they had in common and reply with
all subsequent header hashes.

**Note:** the receiving peer itself may respond with an ```inv``
message <core-ref-p2p-network-data-messages#inv>`__ containing header
hashes of stale blocks. It is up to the requesting peer to poll all of
its peers to find the best block chain.

If the receiving peer does not find a common header hash within the
list, it will assume the last common block was the <> (block zero), so
it will reply with in ```inv``
message <core-ref-p2p-network-data-messages#inv>`__ containing header
hashes starting with block one (the first block after the genesis
block).

+---------+-----------------------+------------------+----------------+
| Bytes   | Name                  | Data Type        | Description    |
+=========+=======================+==================+================+
| 4       | version               | uint32_t         | The protocol   |
|         |                       |                  | version        |
|         |                       |                  | number; the    |
|         |                       |                  | same as sent   |
|         |                       |                  | in the         |
|         |                       |                  | ```version``   |
|         |                       |                  | mes            |
|         |                       |                  | sage <core-ref |
|         |                       |                  | -p2p-network-c |
|         |                       |                  | ontrol-message |
|         |                       |                  | s#version>`__. |
+---------+-----------------------+------------------+----------------+
| *       | hash count            | compactSize uint | The number of  |
| Varies* |                       |                  | header hashes  |
|         |                       |                  | provided not   |
|         |                       |                  | including the  |
|         |                       |                  | stop hash.     |
|         |                       |                  | There is no    |
|         |                       |                  | limit except   |
|         |                       |                  | that the byte  |
|         |                       |                  | size of the    |
|         |                       |                  | entire message |
|         |                       |                  | must be below  |
|         |                       |                  | the            |
|         |                       |                  | ```MAX_SIZE``  |
|         |                       |                  |  <https://gith |
|         |                       |                  | ub.com/dashpay |
|         |                       |                  | /dash/blob/v0. |
|         |                       |                  | 15.x/src/seria |
|         |                       |                  | lize.h#L29>`__ |
|         |                       |                  | limit;         |
|         |                       |                  | typically from |
|         |                       |                  | 1 to 200       |
|         |                       |                  | hashes are     |
|         |                       |                  | sent.          |
+---------+-----------------------+------------------+----------------+
| *       | block header hashes   | char[32]         | One or more    |
| Varies* |                       |                  | block header   |
|         |                       |                  | hashes (32     |
|         |                       |                  | bytes each) in |
|         |                       |                  | internal byte  |
|         |                       |                  | order. Hashes  |
|         |                       |                  | should be      |
|         |                       |                  | provided in    |
|         |                       |                  | reverse order  |
|         |                       |                  | of block       |
|         |                       |                  | height, so     |
|         |                       |                  | highest-height |
|         |                       |                  | hashes are     |
|         |                       |                  | listed first   |
|         |                       |                  | and            |
|         |                       |                  | lowest-height  |
|         |                       |                  | hashes are     |
|         |                       |                  | listed last.   |
+---------+-----------------------+------------------+----------------+
| 32      | stop hash             | char[32]         | The header     |
|         |                       |                  | hash of the    |
|         |                       |                  | last header    |
|         |                       |                  | hash being     |
|         |                       |                  | requested; set |
|         |                       |                  | to all zeroes  |
|         |                       |                  | to request an  |
|         |                       |                  | ```inv``       |
|         |                       |                  | message <      |
|         |                       |                  | core-ref-p2p-n |
|         |                       |                  | etwork-data-me |
|         |                       |                  | ssages#inv>`__ |
|         |                       |                  | with all       |
|         |                       |                  | subsequent     |
|         |                       |                  | header hashes  |
|         |                       |                  | (a maximum of  |
|         |                       |                  | 500 will be    |
|         |                       |                  | sent as a      |
|         |                       |                  | reply to this  |
|         |                       |                  | message; if    |
|         |                       |                  | you need more  |
|         |                       |                  | than 500, you  |
|         |                       |                  | will need to   |
|         |                       |                  | send another   |
|         |                       |                  | ```getblocks`` |
|         |                       |                  | m              |
|         |                       |                  | essage <core-r |
|         |                       |                  | ef-p2p-network |
|         |                       |                  | -data-messages |
|         |                       |                  | #getblocks>`__ |
|         |                       |                  | with a         |
|         |                       |                  | higher-height  |
|         |                       |                  | header hash as |
|         |                       |                  | the first      |
|         |                       |                  | entry in block |
|         |                       |                  | header hash    |
|         |                       |                  | field).        |
+---------+-----------------------+------------------+----------------+

The following annotated hexdump shows a ```getblocks``
message <core-ref-p2p-network-data-messages#getblocks>`__. (The message
header has been omitted.)

.. code:: text

   71110100 ........................... Protocol version: 70001
   02 ................................. Hash count: 2

   d39f608a7775b537729884d4e6633bb2
   105e55a16a14d31b0000000000000000 ... Hash #1

   5c3e6403d40837110a2e8afb602b1c01
   714bda7ce23bea0a0000000000000000 ... Hash #2

   00000000000000000000000000000000
   00000000000000000000000000000000 ... Stop hash

getblocktxn
===========

*Added in protocol version 70209 of Dash Core as described by BIP152*

The ```getblocktxn``
message <core-ref-p2p-network-data-messages#getblocktxn>`__ requests a
```blocktxn`` message <core-ref-p2p-network-data-messages#blocktxn>`__
for any transactions that it has not seen after a compact block is
received. It is defined as a message containing a serialized
``BlockTransactionsRequest`` message. Upon receipt of a
properly-formatted ```getblocktxn``
message <core-ref-p2p-network-data-messages#getblocktxn>`__, <> which
recently provided the sender of such a message with a ```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__ for the block
hash identified in this message must respond with either an appropriate
```blocktxn`` message <core-ref-p2p-network-data-messages#blocktxn>`__,
or a full block message.

A ```blocktxn`` message <core-ref-p2p-network-data-messages#blocktxn>`__
response must contain exactly and only each transaction which is present
in the appropriate block at the index specified in the ```getblocktxn``
message <core-ref-p2p-network-data-messages#getblocktxn>`__ indexes
list, in the order requested.

The structure of ``BlockTransactionsRequest`` is defined below.

+----------+-----------------+-----------------------+----------+-----+
| Bytes    | Name            | Data Type             | Des      |     |
|          |                 |                       | cription |     |
+==========+=================+=======================+==========+=====+
| 32       | blockhash       | Binary blob           | The      | The |
|          |                 |                       | output   | blo |
|          |                 |                       | from a   | ckh |
|          |                 |                       | doubl    | ash |
|          |                 |                       | e-SHA256 | of  |
|          |                 |                       | of the   | the |
|          |                 |                       | block    | bl  |
|          |                 |                       | header,  | ock |
|          |                 |                       | as used  | wh  |
|          |                 |                       | e        | ich |
|          |                 |                       | lsewhere | the |
|          |                 |                       |          | tra |
|          |                 |                       |          | nsa |
|          |                 |                       |          | cti |
|          |                 |                       |          | ons |
|          |                 |                       |          | be  |
|          |                 |                       |          | ing |
|          |                 |                       |          | req |
|          |                 |                       |          | ues |
|          |                 |                       |          | ted |
|          |                 |                       |          | are |
|          |                 |                       |          | in  |
+----------+-----------------+-----------------------+----------+-----+
| *Varies* | indexes_length  | CompactSize uint      | As used  | The |
|          |                 |                       | to       | num |
|          |                 |                       | encode   | ber |
|          |                 |                       | array    | of  |
|          |                 |                       | lengths  | tra |
|          |                 |                       | e        | nsa |
|          |                 |                       | lsewhere | cti |
|          |                 |                       |          | ons |
|          |                 |                       |          | req |
|          |                 |                       |          | ues |
|          |                 |                       |          | ted |
+----------+-----------------+-----------------------+----------+-----+
| *Varies* | indexes         | CompactSize uint[]    | Differ   | Vec |
|          |                 |                       | entially | tor |
|          |                 |                       | encoded  | of  |
|          |                 |                       |          | co  |
|          |                 |                       |          | mpa |
|          |                 |                       |          | ctS |
|          |                 |                       |          | ize |
|          |                 |                       |          | c   |
|          |                 |                       |          | ont |
|          |                 |                       |          | ain |
|          |                 |                       |          | ing |
|          |                 |                       |          | the |
|          |                 |                       |          | i   |
|          |                 |                       |          | nde |
|          |                 |                       |          | xes |
|          |                 |                       |          | of  |
|          |                 |                       |          | the |
|          |                 |                       |          | tra |
|          |                 |                       |          | nsa |
|          |                 |                       |          | cti |
|          |                 |                       |          | ons |
|          |                 |                       |          | be  |
|          |                 |                       |          | ing |
|          |                 |                       |          | req |
|          |                 |                       |          | ues |
|          |                 |                       |          | ted |
|          |                 |                       |          | in  |
|          |                 |                       |          | the |
|          |                 |                       |          | blo |
|          |                 |                       |          | ck. |
+----------+-----------------+-----------------------+----------+-----+

The following annotated hexdump shows a ```getblocktxn``
message <core-ref-p2p-network-data-messages#getblocktxn>`__. (The
message header has been omitted.)

.. code:: text

   182327cb727da7d60541da793831fd0a
   b0509e79c8cd3d654cdf3a0100000000 ... Block Hash

   01 ................................. Index length: 1
   01 ................................. Index: 1

getcfcheckpt
============

*Added in protocol version 70223 of Dash Core as described by*\ `BIP
157 <https://github.com/bitcoin/bips/blob/master/bip-0157.mediawiki>`__\ *.*

The ```getcfcheckpt``
message <core-ref-p2p-network-data-messages#getcfcheckpt>`__ requests
verifiable filter headers for a range of blocks. The response to this
message is a ```cfcheckpt``
message <core-ref-p2p-network-data-messages#cfcheckpt>`__.

+-------+-------------+-----------+---------------------------------+
| Bytes | Name        | Data Type | Description                     |
+=======+=============+===========+=================================+
| 1     | filter_type | uint8_t   | Filter type for which headers   |
|       |             |           | are requested                   |
+-------+-------------+-----------+---------------------------------+
| 32    | stop_hash   | uint256   | The hash of the last block in   |
|       |             |           | the requested range             |
+-------+-------------+-----------+---------------------------------+

getcfheaders
============

*Added in protocol version 70223 of Dash Core as described by*\ `BIP
157 <https://github.com/bitcoin/bips/blob/master/bip-0157.mediawiki>`__\ *.*

The ```getcfheaders``
message <core-ref-p2p-network-data-messages#getcfheaders>`__ requests
verifiable filter headers for a range of blocks. The response to this
message is a ```cfheaders``
message <core-ref-p2p-network-data-messages#cfheaders>`__.

+---------+-----------------------+------------------+----------------+
| Bytes   | Name                  | Data Type        | Description    |
+=========+=======================+==================+================+
| 1       | filter_type           | uint8_t          | Filter type    |
|         |                       |                  | for which      |
|         |                       |                  | headers are    |
|         |                       |                  | requested      |
+---------+-----------------------+------------------+----------------+
| 4       | start_height          | uint32_t         | The height of  |
|         |                       |                  | the first      |
|         |                       |                  | block in the   |
|         |                       |                  | requested      |
|         |                       |                  | range          |
+---------+-----------------------+------------------+----------------+
| 32      | stop_hash             | uint256          | The hash of    |
|         |                       |                  | the last block |
|         |                       |                  | in the         |
|         |                       |                  | requested      |
|         |                       |                  | range. Must be |
|         |                       |                  | >=             |
|         |                       |                  | ``             |
|         |                       |                  | start_height`` |
|         |                       |                  | and the        |
|         |                       |                  | difference     |
|         |                       |                  | between them   |
|         |                       |                  | must be less   |
|         |                       |                  | than 2000.     |
+---------+-----------------------+------------------+----------------+

getcfilters
===========

*Added in protocol version 70223 of Dash Core as described by*\ `BIP
157 <https://github.com/bitcoin/bips/blob/master/bip-0157.mediawiki>`__\ *.*

The ```getcfilters``
message <core-ref-p2p-network-data-messages#getcfilters>`__ requests
compact filters of a particular type for a particular range of blocks.
The response to this message is a ```cfilter``
message <core-ref-p2p-network-data-messages#cfilter>`__.

+---------+-----------------------+------------------+----------------+
| Bytes   | Name                  | Data Type        | Description    |
+=========+=======================+==================+================+
| 1       | filter_type           | uint8_t          | Filter type    |
|         |                       |                  | for which      |
|         |                       |                  | headers are    |
|         |                       |                  | requested      |
+---------+-----------------------+------------------+----------------+
| 4       | start_height          | uint32_t         | The height of  |
|         |                       |                  | the first      |
|         |                       |                  | block in the   |
|         |                       |                  | requested      |
|         |                       |                  | range          |
+---------+-----------------------+------------------+----------------+
| 32      | stop_hash             | uint256          | The hash of    |
|         |                       |                  | the last block |
|         |                       |                  | in the         |
|         |                       |                  | requested      |
|         |                       |                  | range          |
+---------+-----------------------+------------------+----------------+

getdata
=======

The ```getdata`` message <core-ref-p2p-network-data-messages#getdata>`__
requests one or more data objects from another <>. The objects are
requested by an inventory, which the requesting node typically
previously received by way of an ```inv``
message <core-ref-p2p-network-data-messages#inv>`__.

The response to a ```getdata``
message <core-ref-p2p-network-data-messages#getdata>`__ can be a ```tx``
message <core-ref-p2p-network-data-messages#tx>`__, ```block``
message <core-ref-p2p-network-data-messages#block>`__, ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__, ```dstx``
message <core-ref-p2p-network-privatesend-messages#dstx>`__, ```govobj``
message <core-ref-p2p-network-governance-messages#govobj>`__,
```govobjvote``
message <core-ref-p2p-network-governance-messages#govobjvote>`__,
```notfound`` message <core-ref-p2p-network-data-messages#notfound>`__,
```cmpctblock``
message <core-ref-p2p-network-data-messages#cmpctblock>`__, or any other
messages that are exchanged by way of ```inv``
messages <core-ref-p2p-network-data-messages#inv>`__.

This message cannot be used to request arbitrary data, such as historic
transactions no longer in the memory pool or relay set. Full nodes may
not even be able to provide older <> if they’ve pruned old transactions
from their block database. For this reason, the ```getdata``
message <core-ref-p2p-network-data-messages#getdata>`__ should usually
only be used to request data from a node which previously advertised it
had that data by sending an ```inv``
message <core-ref-p2p-network-data-messages#inv>`__.

The format and maximum size limitations of the ```getdata``
message <core-ref-p2p-network-data-messages#getdata>`__ are identical to
the ```inv`` message <core-ref-p2p-network-data-messages#inv>`__; only
the message header differs.

getheaders
==========

*Added in protocol version 70077.*

The ```getheaders``
message <core-ref-p2p-network-data-messages#getheaders>`__ requests a
```headers`` message <core-ref-p2p-network-data-messages#headers>`__
that provides block headers starting from a particular point in the <>.
It allows a <> which has been disconnected or started for the first time
to get the <> it hasn’t seen yet.

The ```getheaders``
message <core-ref-p2p-network-data-messages#getheaders>`__ is nearly
identical to the ```getblocks``
message <core-ref-p2p-network-data-messages#getblocks>`__, with one
minor difference: the ``inv`` reply to the ```getblocks``
message <core-ref-p2p-network-data-messages#getblocks>`__ will include
no more than 500 <> hashes; the ``headers`` reply to the ```getheaders``
message <core-ref-p2p-network-data-messages#getheaders>`__ will include
as many as 2,000 block headers.

+---------+-----------------------+------------------+----------------+
| Bytes   | Name                  | Data Type        | Description    |
+=========+=======================+==================+================+
| 4       | version               | uint32_t         | The protocol   |
|         |                       |                  | version        |
|         |                       |                  | number; the    |
|         |                       |                  | same as sent   |
|         |                       |                  | in the         |
|         |                       |                  | ```version``   |
|         |                       |                  | mes            |
|         |                       |                  | sage <core-ref |
|         |                       |                  | -p2p-network-c |
|         |                       |                  | ontrol-message |
|         |                       |                  | s#version>`__. |
+---------+-----------------------+------------------+----------------+
| *       | hash count            | compactSize uint | The number of  |
| Varies* |                       |                  | header hashes  |
|         |                       |                  | provided not   |
|         |                       |                  | including the  |
|         |                       |                  | stop hash.     |
+---------+-----------------------+------------------+----------------+
| *       | block header hashes   | char[32]         | One or more    |
| Varies* |                       |                  | block header   |
|         |                       |                  | hashes (32     |
|         |                       |                  | bytes each) in |
|         |                       |                  | internal byte  |
|         |                       |                  | order. Hashes  |
|         |                       |                  | should be      |
|         |                       |                  | provided in    |
|         |                       |                  | reverse order  |
|         |                       |                  | of block       |
|         |                       |                  | height, so     |
|         |                       |                  | highest-height |
|         |                       |                  | hashes are     |
|         |                       |                  | listed first   |
|         |                       |                  | and            |
|         |                       |                  | lowest-height  |
|         |                       |                  | hashes are     |
|         |                       |                  | listed last.   |
+---------+-----------------------+------------------+----------------+
| 32      | stop hash             | char[32]         | The header     |
|         |                       |                  | hash of the    |
|         |                       |                  | last header    |
|         |                       |                  | hash being     |
|         |                       |                  | requested; set |
|         |                       |                  | to all zeroes  |
|         |                       |                  | to request as  |
|         |                       |                  | many blocks as |
|         |                       |                  | possible       |
|         |                       |                  | (2000).        |
+---------+-----------------------+------------------+----------------+

getheaders2
===========

*Added in protocol version 70223 of Dash Core.*

The ```getheaders2``
message <core-ref-p2p-network-data-messages#getheaders2>`__ requests a
```headers2`` message <core-ref-p2p-network-data-messages#headers2>`__
that provides block headers starting from a particular point in the <>.
It allows a <> which has been disconnected or started for the first time
to get the <> it hasn’t seen yet.

The ```getheaders2``
message <core-ref-p2p-network-data-messages#getheaders2>`__ contains the
same fields as the ```getheaders``
message <core-ref-p2p-network-data-messages#getheaders>`__.

getmnlistd
==========

*Added in protocol version 70213*

The ```getmnlistd``
message <core-ref-p2p-network-data-messages#getmnlistd>`__ requests a
```mnlistdiff``
message <core-ref-p2p-network-data-messages#mnlistdiff>`__ that provides
either:

1. A full <> list (if ``baseBlockHash`` is all-zero)
2. An update to a previously requested masternode list

+--------------+----------------+-------------+-----------+-----------+
| Bytes        | Name           | Data type   | Required  | De        |
|              |                |             |           | scription |
+==============+================+=============+===========+===========+
| 32           | baseBlockHash  | uint256     | Required  | Hash of a |
|              |                |             |           | block the |
|              |                |             |           | requester |
|              |                |             |           | already   |
|              |                |             |           | has a     |
|              |                |             |           | valid     |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | list      |
|              |                |             |           | of        |
|              |                |             |           | .\ *Note: |
|              |                |             |           | Can be    |
|              |                |             |           | all-zero  |
|              |                |             |           | to        |
|              |                |             |           | indicate  |
|              |                |             |           | that a    |
|              |                |             |           | full      |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | list is   |
|              |                |             |           | re        |
|              |                |             |           | quested.* |
+--------------+----------------+-------------+-----------+-----------+
| 32           | blockHash      | uint256     | Required  | Hash of   |
|              |                |             |           | the block |
|              |                |             |           | for which |
|              |                |             |           | the       |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | list diff |
|              |                |             |           | is        |
|              |                |             |           | requested |
+--------------+----------------+-------------+-----------+-----------+

The following annotated hexdump shows a ```getmnlistd``
message <core-ref-p2p-network-data-messages#getmnlistd>`__. (The message
header has been omitted.)

.. code:: text

   000001ee5108348a2c59396da29dc576
   9b2a9bb303d7577aee9cd95136c49b9b ........... Base block hash

   0000030f51f12e7069a7aa5f1bc9085d
   db3fe368976296fd3b6d73fdaf898cc0 ........... Block hash

getqrinfo
=========

*Added in protocol version 70222 of Dash Core.*

The ``getqrinfo`` message requests a ```qrinfo``
message <core-ref-p2p-network-data-messages#qrinfo>`__ that provides the
information required to verify quorum details for quorums formed using
the quorum rotation process.

+--------------+----------------+-------------+-----------+-----------+
| Bytes        | Name           | Data type   | Required  | De        |
|              |                |             |           | scription |
+==============+================+=============+===========+===========+
| 4            | bas            | uint32      | Required  | Number of |
|              | eBlockHashesNb |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | lists the |
|              |                |             |           | light     |
|              |                |             |           | client    |
|              |                |             |           | already   |
|              |                |             |           | knows (up |
|              |                |             |           | to 4)     |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | b              | uint256[]   | Required  | Array of  |
|              | aseBlockHashes |             |           | base      |
|              |                |             |           | block     |
|              |                |             |           | hashes    |
|              |                |             |           | for the   |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | lists the |
|              |                |             |           | light     |
|              |                |             |           | client    |
|              |                |             |           | already   |
|              |                |             |           | knows     |
+--------------+----------------+-------------+-----------+-----------+
| 32           | bl             | uint256     | Required  | Hash of   |
|              | ockRequestHash |             |           | the block |
|              |                |             |           | for which |
|              |                |             |           | the       |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | list diff |
|              |                |             |           | is        |
|              |                |             |           | requested |
+--------------+----------------+-------------+-----------+-----------+
| 1            | extraShare     | bool        | Optional  | Flag to   |
|              |                |             |           | indicate  |
|              |                |             |           | if an     |
|              |                |             |           | extra     |
|              |                |             |           | share is  |
|              |                |             |           | requested |
+--------------+----------------+-------------+-----------+-----------+

The following annotated hexdump shows a ```getqrinfo``
message <core-ref-p2p-network-data-messages#getqrinfo>`__. (The message
header has been omitted.)

.. code:: text

   06000000 ........................... Number of base block hashes: 6

   06 ................................. Number of base block hashes: 6

   8157c193543166dfc23a67fab1378bc5
   1ded183b09c6e932e64a4639d9010000 ... Base block hash 1
   e173b01943fe7b8d2bf5a13c034eafed
   d2708a1a0f9e5104b86439382e050000 ... Base block hash 2
   c2e276c518b3f8484b237e300094e222
   388a095f486c822585a67b1520010000 ... Base block hash 3
   044822e5c6d568af03422edc00ad3ae5
   029a022139e37d06ae587169c8000000 ... Base block hash 4
   60b2c8280d6e59cdee4c65d7fd352c25
   50bb40242fc242bf091872c1bb010000 ... Base block hash 5
   55d37e40ae54e536ccabf964a403c921
   f021a2b0ca82a6ba63022015a8010000 ... Base block hash 6

   847b42ae4509c82e8c8ba599f23f15c1
   e34c899a09e4ebf440f6e2ef4b000000 ... Block request hash

   01 ................................. Extra share: true

headers
=======

*Added in protocol version 31800 (of Bitcoin).*

The ```headers`` message <core-ref-p2p-network-data-messages#headers>`__
sends block headers to a <> which previously requested certain <> with a
```getheaders``
message <core-ref-p2p-network-data-messages#getheaders>`__. A headers
message can be empty.

+------------+-----------+-----------------------+---------------------+
| Bytes      | Name      | Data Type             | Description         |
+============+===========+=======================+=====================+
| *Varies*   | count     | compactSize uint      | Number of block     |
|            |           |                       | headers up to a     |
|            |           |                       | maximum of 2,000.   |
|            |           |                       | Note: headers-first |
|            |           |                       | sync assumes the    |
|            |           |                       | sending node will   |
|            |           |                       | send the maximum    |
|            |           |                       | number of headers   |
|            |           |                       | whenever possible.  |
+------------+-----------+-----------------------+---------------------+
| *Varies*   | headers   | block_header          | Block headers: each |
|            |           |                       | 80-byte block       |
|            |           |                       | header is in the    |
|            |           |                       | format described in |
|            |           |                       | the `block headers  |
|            |           |                       | section <           |
|            |           |                       | core-ref-block-chai |
|            |           |                       | n-block-headers>`__ |
|            |           |                       | with an additional  |
|            |           |                       | 0x00 suffixed. This |
|            |           |                       | 0x00 is called the  |
|            |           |                       | transaction count,  |
|            |           |                       | but because the     |
|            |           |                       | headers message     |
|            |           |                       | doesn’t include any |
|            |           |                       | transactions, the   |
|            |           |                       | transaction count   |
|            |           |                       | is always zero.     |
+------------+-----------+-----------------------+---------------------+

The following annotated hexdump shows a ```headers``
message <core-ref-p2p-network-data-messages#headers>`__. (The message
header has been omitted.)

.. code:: text

   01 ................................. Header count: 1

   02000000 ........................... Block version: 2
   b6ff0b1b1680a2862a30ca44d346d9e8
   910d334beb48ca0c0000000000000000 ... Hash of previous block's header
   9d10aa52ee949386ca9385695f04ede2
   70dda20810decd12bc9b048aaab31471 ... Merkle root
   24d95a54 ........................... Unix time: 1415239972
   30c31b18 ........................... Target (bits)
   fe9f0864 ........................... Nonce

   00 ................................. Transaction count (0x00)

headers2
========

*Added in protocol version 70223 of Dash Core.*

The ```headers2``
message <core-ref-p2p-network-data-messages#headers2>`__ sends
compressed block headers to a <> which previously requested certain <>
with a ```getheaders2``
message <core-ref-p2p-network-data-messages#getheaders>`__ or indicated
it wants to receive them by signaling with a ```sendheaders2``
message <core-ref-p2p-network-control-messages#sendheaders2>`__.

+------------+-----------+-----------------------+---------------------+
| Bytes      | Name      | Data Type             | Description         |
+============+===========+=======================+=====================+
| *Varies*   | count     | compactSize uint      | Number of block     |
|            |           |                       | headers up to a     |
|            |           |                       | maximum of 2,000.   |
|            |           |                       | Note: headers-first |
|            |           |                       | sync assumes the    |
|            |           |                       | sending node will   |
|            |           |                       | send the maximum    |
|            |           |                       | number of headers   |
|            |           |                       | whenever possible.  |
+------------+-----------+-----------------------+---------------------+
| *Varies*   | headers   | block_header2         | Block headers in    |
|            |           |                       | the                 |
|            |           |                       | ```block_he         |
|            |           |                       | ader2`` <https://gi |
|            |           |                       | thub.com/thephez/di |
|            |           |                       | ps/blob/compressed- |
|            |           |                       | headers/compressed- |
|            |           |                       | headers.md#block_he |
|            |           |                       | ader2-data-type>`__ |
|            |           |                       | format\ **Note**:   |
|            |           |                       | the first header    |
|            |           |                       | will always be      |
|            |           |                       | uncompressed.       |
+------------+-----------+-----------------------+---------------------+

The following annotated hexdump shows a ```headers2``
message <core-ref-p2p-network-data-messages#headers2>`__. (The message
header has been omitted.)

.. code:: text

   fdd007 ............................. Header count: 2000 (0x07d0)

   Header 1 (uncompressed)
   | 38 ............................... Bitfield (0x00100110)
   | 02000000 ......................... Block version
   | 2cbcf83b62913d56f605c0e581a48872
   | 839428c92e5eb76cd7ad94bcaf0b0000 . Hash of previous block's header
   | 7f11dcce14075520e8f74cc4ddf092b4
   | e26ebd23b8d8665a1ae5bfc41b58fdb4 . Merkle root
   | c3a95e53 ......................... Unix time: 1398712771
   | ffff0f1e ......................... Target (bits)
   | f37a0000 ......................... Nonce

   Header 2 (previous header and time compressed)
   | 20 ............................... Bitfield (0x00100000)
   | 02000000 ......................... Block version
   | .................................. Hash of previous block's header (compressed)
   | 4d29e4f9b2e05a9ac97dd5ae4128b3c0
   | 104bdc95aabaa566cc8eeb682e336d0d . Merkle root
   | 0100 ............................. Unix time (compressed)
   | f0ff0f1e ......................... Target (bits)
   | 7b190000 ......................... Nonce

   Header 3 (block version, previous header, time, and bits compressed)
   | 01 ............................... Bitfield (0x00000001)
   | .................................. Block version (compressed)
   | .................................. Hash of previous block's header (compressed)
   | 58968247522cc488db01996de610d6f3
   | b8c348e748198dc528a305941211c71c . Merkle root
   | 0200 ............................. Unix time (compressed)
   | .................................. Target (bits) (compressed)
   | 7b190000 ......................... Nonce

   ... Remaining headers truncated

inv
===

The ```inv`` message <core-ref-p2p-network-data-messages#inv>`__
(inventory message) transmits one or more <> of objects known to the
transmitting <>. It can be sent unsolicited to announce new <> or <>, or
it can be sent in reply to a ```getblocks``
message <core-ref-p2p-network-data-messages#getblocks>`__ or
```mempool`` message <core-ref-p2p-network-data-messages#mempool>`__.

The receiving peer can compare the inventories from an ```inv``
message <core-ref-p2p-network-data-messages#inv>`__ against the
inventories it has already seen, and then use a follow-up message to
request unseen objects.

+----------+-----------+--------------------------+-------------------+
| Bytes    | Name      | Data Type                | Description       |
+==========+===========+==========================+===================+
| *Varies* | count     | compactSize uint         | The number of     |
|          |           |                          | inventory         |
|          |           |                          | entries.          |
+----------+-----------+--------------------------+-------------------+
| *Varies* | inventory | inventory                | One or more       |
|          |           |                          | inventory entries |
|          |           |                          | up to a maximum   |
|          |           |                          | of 50,000         |
|          |           |                          | entries.          |
+----------+-----------+--------------------------+-------------------+

The following annotated hexdump shows an ```inv``
message <core-ref-p2p-network-data-messages#inv>`__ with two inventory
entries. (The message header has been omitted.)

.. code:: text

   02 ................................. Count: 2

   0f000000 ........................... Type: MSG_MASTERNODE_PING
   dd6cc6c11211793b239c2e311f1496e2
   2281b200b35233eaae465d2aa3c9d537 ... Hash (mnp)

   05000000 ........................... Type: MSG_TXLOCK_VOTE
   afc5b2f418f8c06c477a7d071240f5ee
   ab17057f9ce4b50c2aef4fadf3729a2e ... Hash (txlvote)

mempool
=======

*Added in protocol version 60002 (of Bitcoin).*

The ```mempool`` message <core-ref-p2p-network-data-messages#mempool>`__
requests the <> of transactions that the receiving <> has verified as
valid but which have not yet appeared in a <>. That is, transactions
which are in the receiving node’s memory pool. The response to the
```mempool`` message <core-ref-p2p-network-data-messages#mempool>`__ is
one or more ```inv``
messages <core-ref-p2p-network-data-messages#inv>`__ containing the
TXIDs in the usual <> format.

Sending the ```mempool``
message <core-ref-p2p-network-data-messages#mempool>`__ is mostly useful
when a program first connects to the network. Full nodes can use it to
quickly gather most or all of the unconfirmed transactions available on
the network; this is especially useful for miners trying to gather
transactions for their transaction fees. SPV clients can set a filter
before sending a ``mempool`` to only receive transactions that match
that filter; this allows a recently-started client to get most or all
unconfirmed transactions related to its wallet. [block:callout] {
“type”: “info”, “body”: “Dash Core 0.15.0 expanded the mempool message
to include syncing of `InstantSend
Lock <core-ref-p2p-network-instantsend-messages#islock>`__ inventories.
Additionally, nodes now attempt to sync their mempool with peers at
startup by default (limited to peers using protocol version 70216 or
higher). This allows nodes to more quickly detect any double-spend
attempts as well as show InstantSend lock status correctly for
transactions received while
offline.:raw-latex:`\n`:raw-latex:`\nDash `Core 0.17.0 expanded the
mempool message to include syncing of
`ChainLock <core-ref-p2p-network-instantsend-messages#clsig>`__
inventories. This allows nodes to more quickly show ChainLock status
correctly after being offline.”, “title”: “InstantSend and ChainLock
Synchronization” } [/block] The ``inv`` response to the ```mempool``
message <core-ref-p2p-network-data-messages#mempool>`__ is, at best, one
node’s view of the network—not a complete list of every <> on the
network. Here are some additional reasons the list might not be
complete:

-  The ```mempool``
   message <core-ref-p2p-network-data-messages#mempool>`__ is not
   currently fully compatible with the ```filterload``
   message <core-ref-p2p-network-control-messages#filterload>`__\ ’s
   ``BLOOM_UPDATE_ALL`` and ``BLOOM_UPDATE_P2PUBKEY_ONLY`` flags.
   Mempool transactions are not sorted like in-block transactions, so a
   transaction (tx2) spending an <> can appear before the transaction
   (tx1) containing that output, which means the automatic filter update
   mechanism won’t operate until the second-appearing transaction (tx1)
   is seen—missing the first-appearing transaction (tx2). It has been
   proposed in `Bitcoin Core issue
   #2381 <https://github.com/bitcoin/bitcoin/issues/2381>`__ that the
   transactions should be sorted before being processed by the filter.

There is no payload in a ```mempool``
message <core-ref-p2p-network-data-messages#mempool>`__. See the
`message header section <core-ref-p2p-network-message-headers>`__ for an
example of a message without a payload.

merkleblock
===========

*Added in protocol version 70001 as described by BIP37.*

The ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ is a reply
to a ```getdata``
message <core-ref-p2p-network-data-messages#getdata>`__ which requested
a <> using the inventory type ``MSG_MERKLEBLOCK``. It is only part of
the reply: if any matching transactions are found, they will be sent
separately as ```tx``
messages <core-ref-p2p-network-data-messages#tx>`__. As of Dash Core
0.17.0 ```islock``
messages <core-ref-p2p-network-instantsend-messages#islock>`__ for
matching transactions are sent if present. [block:callout] { “type”:
“warning”, “body”: “Note: ``islock`` messages are currently dropped once
a ChainLock is present so in most cases they will not actually be
provided in response to a merkleblock request. Future updates may modify
this behavior.” } [/block] If a filter has been previously set with the
```filterload``
message <core-ref-p2p-network-control-messages#filterload>`__, the
```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ will contain
the <> of any transactions in the requested block that matched the
filter, as well as any parts of the block’s <> necessary to connect
those transactions to the block header’s <>. The message also contains a
complete copy of the <> to allow the client to hash it and confirm its
<>.

+----------+---------------------+-------------------+-----------------+
| Bytes    | Name                | Data Type         | Description     |
+==========+=====================+===================+=================+
| 80       | block header        | block_header      | The block       |
|          |                     |                   | header in the   |
|          |                     |                   | format          |
|          |                     |                   | described in    |
|          |                     |                   | the `block      |
|          |                     |                   | header          |
|          |                     |                   | sec             |
|          |                     |                   | tion <core-ref- |
|          |                     |                   | block-chain-blo |
|          |                     |                   | ck-headers>`__. |
+----------+---------------------+-------------------+-----------------+
| 4        | transaction count   | uint32_t          | The number of   |
|          |                     |                   | transactions in |
|          |                     |                   | the block       |
|          |                     |                   | (including ones |
|          |                     |                   | that don’t      |
|          |                     |                   | match the       |
|          |                     |                   | filter).        |
+----------+---------------------+-------------------+-----------------+
| *Varies* | hash count          | compactSize uint  | The number of   |
|          |                     |                   | hashes in the   |
|          |                     |                   | following       |
|          |                     |                   | field.          |
+----------+---------------------+-------------------+-----------------+
| *Varies* | hashes              | char[32]          | One or more     |
|          |                     |                   | hashes of both  |
|          |                     |                   | transactions    |
|          |                     |                   | and merkle      |
|          |                     |                   | nodes in        |
|          |                     |                   | internal byte   |
|          |                     |                   | order. Each     |
|          |                     |                   | hash is 32      |
|          |                     |                   | bytes.          |
+----------+---------------------+-------------------+-----------------+
| *Varies* | flag byte count     | compactSize uint  | The number of   |
|          |                     |                   | flag bytes in   |
|          |                     |                   | the following   |
|          |                     |                   | field.          |
+----------+---------------------+-------------------+-----------------+
| *Varies* | flags               | byte[]            | A sequence of   |
|          |                     |                   | bits packed     |
|          |                     |                   | eight in a byte |
|          |                     |                   | with the least  |
|          |                     |                   | significant bit |
|          |                     |                   | first. May be   |
|          |                     |                   | padded to the   |
|          |                     |                   | nearest byte    |
|          |                     |                   | boundary but    |
|          |                     |                   | must not        |
|          |                     |                   | contain any     |
|          |                     |                   | more bits than  |
|          |                     |                   | that. Used to   |
|          |                     |                   | assign the      |
|          |                     |                   | hashes to       |
|          |                     |                   | particular      |
|          |                     |                   | nodes in the    |
|          |                     |                   | merkle tree as  |
|          |                     |                   | described       |
|          |                     |                   | below.          |
+----------+---------------------+-------------------+-----------------+

The annotated hexdump below shows a ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ which
corresponds to the examples below. (The message header has been
omitted.)

.. code:: text

   01000000 ........................... Block version: 1
   82bb869cf3a793432a66e826e05a6fc3
   7469f8efb7421dc88067010000000000 ... Hash of previous block's header
   7f16c5962e8bd963659c793ce370d95f
   093bc7e367117b3c30c1f8fdd0d97287 ... Merkle root
   76381b4d ........................... Time: 1293629558
   4c86041b ........................... nBits: 0x04864c * 256**(0x1b-3)
   554b8529 ........................... Nonce

   07000000 ........................... Transaction count: 7
   04 ................................. Hash count: 4

   3612262624047ee87660be1a707519a4
   43b1c1ce3d248cbfc6c15870f6c5daa2 ... Hash #1
   019f5b01d4195ecbc9398fbf3c3b1fa9
   bb3183301d7a1fb3bd174fcfa40a2b65 ... Hash #2
   41ed70551dd7e841883ab8f0b16bf041
   76b7d1480e4f0af9f3d4c3595768d068 ... Hash #3
   20d2a7bc994987302e5b1ac80fc425fe
   25f8b63169ea78e68fbaaefa59379bbf ... Hash #4

   01 ................................. Flag bytes: 1
   1d ................................. Flags: 1 0 1 1 1 0 0 0

Note: when fully decoded, the above ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ provided the
TXID for a single transaction that matched the filter. In the <> traffic
dump this output was taken from, the full transaction belonging to that
TXID was sent immediately after the ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ as a ```tx``
message <core-ref-p2p-network-data-messages#tx>`__.

Parsing A MerkleBlock Message
-----------------------------

As seen in the annotated hexdump above, the ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ provides
three special data types: a transaction count, a list of hashes, and a
list of one-bit flags.

You can use the transaction count to construct an empty <>. We’ll call
each entry in the tree a node; on the bottom are TXID nodes—the hashes
for these nodes are <>; the remaining nodes (including the <>) are
non-TXID nodes—they may actually have the same hash as a TXID, but we
treat them differently.

.. figure:: https://dash-docs.github.io/img/dev/animated-en-merkleblock-parsing.gif
   :alt: Example Of Parsing A MerkleBlock Message

   Example Of Parsing A MerkleBlock Message

Keep the hashes and flags in the order they appear in the
```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__. When we say
“next flag” or “next hash”, we mean the next flag or hash on the list,
even if it’s the first one we’ve used so far.

Start with the merkle root node and the first flag. The table below
describes how to evaluate a flag based on whether the node being
processed is a TXID node or a non-TXID node. Once you apply a flag to a
node, never apply another flag to that same node or reuse that same flag
again.

+---+---------------------------------------------------------------+---+
| F | TXID Node                                                     | N |
| l |                                                               | o |
| a |                                                               | n |
| g |                                                               | - |
|   |                                                               | T |
|   |                                                               | X |
|   |                                                               | I |
|   |                                                               | D |
|   |                                                               | N |
|   |                                                               | o |
|   |                                                               | d |
|   |                                                               | e |
+===+===============================================================+===+
| * | Use the next hash as this node’s TXID, but this transaction   | U |
| * | didn’t match the filter.                                      | s |
| 0 |                                                               | e |
| * |                                                               | t |
| * |                                                               | h |
|   |                                                               | e |
|   |                                                               | n |
|   |                                                               | e |
|   |                                                               | x |
|   |                                                               | t |
|   |                                                               | h |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | t |
|   |                                                               | h |
|   |                                                               | i |
|   |                                                               | s |
|   |                                                               | n |
|   |                                                               | o |
|   |                                                               | d |
|   |                                                               | e |
|   |                                                               | ’ |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | . |
|   |                                                               | D |
|   |                                                               | o |
|   |                                                               | n |
|   |                                                               | ’ |
|   |                                                               | t |
|   |                                                               | p |
|   |                                                               | r |
|   |                                                               | o |
|   |                                                               | c |
|   |                                                               | e |
|   |                                                               | s |
|   |                                                               | s |
|   |                                                               | a |
|   |                                                               | n |
|   |                                                               | y |
|   |                                                               | d |
|   |                                                               | e |
|   |                                                               | s |
|   |                                                               | c |
|   |                                                               | e |
|   |                                                               | n |
|   |                                                               | d |
|   |                                                               | a |
|   |                                                               | n |
|   |                                                               | t |
|   |                                                               | n |
|   |                                                               | o |
|   |                                                               | d |
|   |                                                               | e |
|   |                                                               | s |
|   |                                                               | . |
+---+---------------------------------------------------------------+---+
| * | Use the next hash as this node’s TXID, and mark this          | T |
| * | transaction as matching the filter.                           | h |
| 1 |                                                               | e |
| * |                                                               | h |
| * |                                                               | a |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | n |
|   |                                                               | e |
|   |                                                               | e |
|   |                                                               | d |
|   |                                                               | s |
|   |                                                               | t |
|   |                                                               | o |
|   |                                                               | b |
|   |                                                               | e |
|   |                                                               | c |
|   |                                                               | o |
|   |                                                               | m |
|   |                                                               | p |
|   |                                                               | u |
|   |                                                               | t |
|   |                                                               | e |
|   |                                                               | d |
|   |                                                               | . |
|   |                                                               | P |
|   |                                                               | r |
|   |                                                               | o |
|   |                                                               | c |
|   |                                                               | e |
|   |                                                               | s |
|   |                                                               | s |
|   |                                                               | t |
|   |                                                               | h |
|   |                                                               | e |
|   |                                                               | l |
|   |                                                               | e |
|   |                                                               | f |
|   |                                                               | t |
|   |                                                               | c |
|   |                                                               | h |
|   |                                                               | i |
|   |                                                               | l |
|   |                                                               | d |
|   |                                                               | n |
|   |                                                               | o |
|   |                                                               | d |
|   |                                                               | e |
|   |                                                               | t |
|   |                                                               | o |
|   |                                                               | g |
|   |                                                               | e |
|   |                                                               | t |
|   |                                                               | i |
|   |                                                               | t |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | ; |
|   |                                                               | p |
|   |                                                               | r |
|   |                                                               | o |
|   |                                                               | c |
|   |                                                               | e |
|   |                                                               | s |
|   |                                                               | s |
|   |                                                               | t |
|   |                                                               | h |
|   |                                                               | e |
|   |                                                               | r |
|   |                                                               | i |
|   |                                                               | g |
|   |                                                               | h |
|   |                                                               | t |
|   |                                                               | c |
|   |                                                               | h |
|   |                                                               | i |
|   |                                                               | l |
|   |                                                               | d |
|   |                                                               | n |
|   |                                                               | o |
|   |                                                               | d |
|   |                                                               | e |
|   |                                                               | t |
|   |                                                               | o |
|   |                                                               | g |
|   |                                                               | e |
|   |                                                               | t |
|   |                                                               | i |
|   |                                                               | t |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | ; |
|   |                                                               | t |
|   |                                                               | h |
|   |                                                               | e |
|   |                                                               | n |
|   |                                                               | c |
|   |                                                               | o |
|   |                                                               | n |
|   |                                                               | c |
|   |                                                               | a |
|   |                                                               | t |
|   |                                                               | e |
|   |                                                               | n |
|   |                                                               | a |
|   |                                                               | t |
|   |                                                               | e |
|   |                                                               | t |
|   |                                                               | h |
|   |                                                               | e |
|   |                                                               | t |
|   |                                                               | w |
|   |                                                               | o |
|   |                                                               | h |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | e |
|   |                                                               | s |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | 6 |
|   |                                                               | 4 |
|   |                                                               | r |
|   |                                                               | a |
|   |                                                               | w |
|   |                                                               | b |
|   |                                                               | y |
|   |                                                               | t |
|   |                                                               | e |
|   |                                                               | s |
|   |                                                               | a |
|   |                                                               | n |
|   |                                                               | d |
|   |                                                               | h |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | t |
|   |                                                               | h |
|   |                                                               | e |
|   |                                                               | m |
|   |                                                               | t |
|   |                                                               | o |
|   |                                                               | g |
|   |                                                               | e |
|   |                                                               | t |
|   |                                                               | t |
|   |                                                               | h |
|   |                                                               | i |
|   |                                                               | s |
|   |                                                               | n |
|   |                                                               | o |
|   |                                                               | d |
|   |                                                               | e |
|   |                                                               | ’ |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | a |
|   |                                                               | s |
|   |                                                               | h |
|   |                                                               | . |
+---+---------------------------------------------------------------+---+

Any time you begin processing a node for the first time, evaluate the
next flag. Never use a flag at any other time.

When processing a child node, you may need to process its children (the
grandchildren of the original node) or further-descended nodes before
returning to the parent node. This is expected—keep processing depth
first until you reach a TXID node or a non-TXID node with a flag of 0.

After you process a TXID node or a non-TXID node with a flag of 0, stop
processing flags and begin to ascend the tree. As you ascend, compute
the hash of any nodes for which you now have both child hashes or for
which you now have the sole child hash. See the `merkle tree
section <core-ref-block-chain-block-headers#merkle-trees>`__ for hashing
instructions. If you reach a node where only the left hash is known,
descend into its right child (if present) and further descendants as
necessary.

However, if you find a node whose left and right children both have the
same hash, fail. This is related to CVE-2012-2459.

Continue descending and ascending until you have enough information to
obtain the hash of the merkle root node. If you run out of flags or
hashes before that condition is reached, fail. Then perform the
following checks (order doesn’t matter):

-  Fail if there are unused hashes in the hashes list.

-  Fail if there are unused flag bits—except for the minimum number of
   bits necessary to pad up to the next full byte.

-  Fail if the hash of the merkle root node is not identical to the
   merkle root in the <>.

-  Fail if the block header is invalid. Remember to ensure that the hash
   of the header is less than or equal to the <> encoded by the nBits
   header field. Your program should also, of course, attempt to ensure
   the header belongs to the best block chain and that the user knows
   how many confirmations this block has.

For a detailed example of parsing a ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__, please see
the corresponding `merkle block examples
section <core-example-p2p-network-parsing-a-merkleblock>`__.

Creating A MerkleBlock Message
------------------------------

It’s easier to understand how to create a ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ after you
understand how to parse an already-created message, so we recommend you
read the parsing section above first.

Create a complete merkle tree with <> on the bottom row and all the
other hashes calculated up to the <> on the top row. For each
transaction that matches the filter, track its TXID node and all of its
ancestor nodes.

.. figure:: https://dash-docs.github.io/img/dev/animated-en-merkleblock-creation.gif
   :alt: Example Of Creating A MerkleBlock Message

   Example Of Creating A MerkleBlock Message

Start processing the tree with the <> node. The table below describes
how to process both TXID nodes and non-TXID nodes based on whether the
node is a match, a match ancestor, or neither a match nor a match
ancestor.

+-----------------------+--------------------------------------------+---+
|                       | TXID Node                                  | N |
|                       |                                            | o |
|                       |                                            | n |
|                       |                                            | - |
|                       |                                            | T |
|                       |                                            | X |
|                       |                                            | I |
|                       |                                            | D |
|                       |                                            | N |
|                       |                                            | o |
|                       |                                            | d |
|                       |                                            | e |
+=======================+============================================+===+
| **Neither Match Nor   | Append a 0 to the flag list; append this   | A |
| Match Ancestor**      | node’s TXID to the hash list.              | p |
|                       |                                            | p |
|                       |                                            | e |
|                       |                                            | n |
|                       |                                            | d |
|                       |                                            | a |
|                       |                                            | 0 |
|                       |                                            | t |
|                       |                                            | o |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | e |
|                       |                                            | f |
|                       |                                            | l |
|                       |                                            | a |
|                       |                                            | g |
|                       |                                            | l |
|                       |                                            | i |
|                       |                                            | s |
|                       |                                            | t |
|                       |                                            | ; |
|                       |                                            | a |
|                       |                                            | p |
|                       |                                            | p |
|                       |                                            | e |
|                       |                                            | n |
|                       |                                            | d |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | i |
|                       |                                            | s |
|                       |                                            | n |
|                       |                                            | o |
|                       |                                            | d |
|                       |                                            | e |
|                       |                                            | ’ |
|                       |                                            | s |
|                       |                                            | h |
|                       |                                            | a |
|                       |                                            | s |
|                       |                                            | h |
|                       |                                            | t |
|                       |                                            | o |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | e |
|                       |                                            | h |
|                       |                                            | a |
|                       |                                            | s |
|                       |                                            | h |
|                       |                                            | l |
|                       |                                            | i |
|                       |                                            | s |
|                       |                                            | t |
|                       |                                            | . |
|                       |                                            | D |
|                       |                                            | o |
|                       |                                            | n |
|                       |                                            | o |
|                       |                                            | t |
|                       |                                            | d |
|                       |                                            | e |
|                       |                                            | s |
|                       |                                            | c |
|                       |                                            | e |
|                       |                                            | n |
|                       |                                            | d |
|                       |                                            | i |
|                       |                                            | n |
|                       |                                            | t |
|                       |                                            | o |
|                       |                                            | i |
|                       |                                            | t |
|                       |                                            | s |
|                       |                                            | c |
|                       |                                            | h |
|                       |                                            | i |
|                       |                                            | l |
|                       |                                            | d |
|                       |                                            | n |
|                       |                                            | o |
|                       |                                            | d |
|                       |                                            | e |
|                       |                                            | s |
|                       |                                            | . |
+-----------------------+--------------------------------------------+---+
| **Match Or Match      | Append a 1 to the flag list; append this   | A |
| Ancestor**            | node’s TXID to the hash list.              | p |
|                       |                                            | p |
|                       |                                            | e |
|                       |                                            | n |
|                       |                                            | d |
|                       |                                            | a |
|                       |                                            | 1 |
|                       |                                            | t |
|                       |                                            | o |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | e |
|                       |                                            | f |
|                       |                                            | l |
|                       |                                            | a |
|                       |                                            | g |
|                       |                                            | l |
|                       |                                            | i |
|                       |                                            | s |
|                       |                                            | t |
|                       |                                            | ; |
|                       |                                            | p |
|                       |                                            | r |
|                       |                                            | o |
|                       |                                            | c |
|                       |                                            | e |
|                       |                                            | s |
|                       |                                            | s |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | e |
|                       |                                            | l |
|                       |                                            | e |
|                       |                                            | f |
|                       |                                            | t |
|                       |                                            | c |
|                       |                                            | h |
|                       |                                            | i |
|                       |                                            | l |
|                       |                                            | d |
|                       |                                            | n |
|                       |                                            | o |
|                       |                                            | d |
|                       |                                            | e |
|                       |                                            | . |
|                       |                                            | T |
|                       |                                            | h |
|                       |                                            | e |
|                       |                                            | n |
|                       |                                            | , |
|                       |                                            | i |
|                       |                                            | f |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | e |
|                       |                                            | n |
|                       |                                            | o |
|                       |                                            | d |
|                       |                                            | e |
|                       |                                            | h |
|                       |                                            | a |
|                       |                                            | s |
|                       |                                            | a |
|                       |                                            | r |
|                       |                                            | i |
|                       |                                            | g |
|                       |                                            | h |
|                       |                                            | t |
|                       |                                            | c |
|                       |                                            | h |
|                       |                                            | i |
|                       |                                            | l |
|                       |                                            | d |
|                       |                                            | , |
|                       |                                            | p |
|                       |                                            | r |
|                       |                                            | o |
|                       |                                            | c |
|                       |                                            | e |
|                       |                                            | s |
|                       |                                            | s |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | e |
|                       |                                            | r |
|                       |                                            | i |
|                       |                                            | g |
|                       |                                            | h |
|                       |                                            | t |
|                       |                                            | c |
|                       |                                            | h |
|                       |                                            | i |
|                       |                                            | l |
|                       |                                            | d |
|                       |                                            | . |
|                       |                                            | D |
|                       |                                            | o |
|                       |                                            | n |
|                       |                                            | o |
|                       |                                            | t |
|                       |                                            | a |
|                       |                                            | p |
|                       |                                            | p |
|                       |                                            | e |
|                       |                                            | n |
|                       |                                            | d |
|                       |                                            | a |
|                       |                                            | h |
|                       |                                            | a |
|                       |                                            | s |
|                       |                                            | h |
|                       |                                            | t |
|                       |                                            | o |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | e |
|                       |                                            | h |
|                       |                                            | a |
|                       |                                            | s |
|                       |                                            | h |
|                       |                                            | l |
|                       |                                            | i |
|                       |                                            | s |
|                       |                                            | t |
|                       |                                            | f |
|                       |                                            | o |
|                       |                                            | r |
|                       |                                            | t |
|                       |                                            | h |
|                       |                                            | i |
|                       |                                            | s |
|                       |                                            | n |
|                       |                                            | o |
|                       |                                            | d |
|                       |                                            | e |
|                       |                                            | . |
+-----------------------+--------------------------------------------+---+

Any time you begin processing a node for the first time, a flag should
be appended to the flag list. Never put a flag on the list at any other
time, except when processing is complete to pad out the flag list to a
byte boundary.

When processing a child node, you may need to process its children (the
grandchildren of the original node) or further-descended nodes before
returning to the parent node. This is expected—keep processing depth
first until you reach a TXID node or a node which is neither a TXID nor
a match ancestor.

After you process a TXID node or a node which is neither a TXID nor a
match ancestor, stop processing and begin to ascend the tree until you
find a node with a right child you haven’t processed yet. Descend into
that right child and process it.

After you fully process the merkle root node according to the
instructions in the table above, processing is complete. Pad your flag
list to a byte boundary and construct the ```merkleblock``
message <core-ref-p2p-network-data-messages#merkleblock>`__ using the
template near the beginning of this subsection.

mnlistdiff
==========

*Added in protocol version 70213*

The ```mnlistdiff``
message <core-ref-p2p-network-data-messages#mnlistdiff>`__ is a reply to
a ```getmnlistd``
message <core-ref-p2p-network-data-messages#getmnlistd>`__ which
requested either a full <> list or a diff for a range of <>.

+--------------+----------------+-------------+-----------+-----------+
| Bytes        | Name           | Datatype    | Required  | De        |
|              |                |             |           | scription |
+==============+================+=============+===========+===========+
| 32           | baseBlockHash  | uint256     | Required  | Hash of a |
|              |                |             |           | block the |
|              |                |             |           | requester |
|              |                |             |           | already   |
|              |                |             |           | has a     |
|              |                |             |           | valid     |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | list of.  |
|              |                |             |           | Can be    |
|              |                |             |           | all-zero  |
|              |                |             |           | to        |
|              |                |             |           | indicate  |
|              |                |             |           | that a    |
|              |                |             |           | full      |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | list is   |
|              |                |             |           | r         |
|              |                |             |           | equested. |
+--------------+----------------+-------------+-----------+-----------+
| 32           | blockHash      | uint256     | Required  | Hash of   |
|              |                |             |           | the block |
|              |                |             |           | for which |
|              |                |             |           | the       |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | list diff |
|              |                |             |           | is        |
|              |                |             |           | requested |
+--------------+----------------+-------------+-----------+-----------+
| 4            | tot            | uint32_t    | Required  | Number of |
|              | alTransactions |             |           | total     |
|              |                |             |           | tra       |
|              |                |             |           | nsactions |
|              |                |             |           | in        |
|              |                |             |           | ``bl      |
|              |                |             |           | ockHash`` |
+--------------+----------------+-------------+-----------+-----------+
| 1-9          | mer            | compactSize | Required  | Number of |
|              | kleHashesCount | uint        |           | Merkle    |
|              |                |             |           | hashes    |
+--------------+----------------+-------------+-----------+-----------+
| variable     | merkleHashes   | vector      | Required  | Merkle    |
|              |                |             |           | hashes in |
|              |                |             |           | de        |
|              |                |             |           | pth-first |
|              |                |             |           | order     |
+--------------+----------------+-------------+-----------+-----------+
| 1-9          | me             | compactSize | Required  | Number of |
|              | rkleFlagsCount | uint        |           | Merkle    |
|              |                |             |           | flag      |
|              |                |             |           | bytes     |
+--------------+----------------+-------------+-----------+-----------+
| variable     | merkleFlags    | vector      | Required  | Merkle    |
|              |                |             |           | flag      |
|              |                |             |           | bits,     |
|              |                |             |           | packed    |
|              |                |             |           | per 8 in  |
|              |                |             |           | a byte,   |
|              |                |             |           | least     |
|              |                |             |           | si        |
|              |                |             |           | gnificant |
|              |                |             |           | bit first |
+--------------+----------------+-------------+-----------+-----------+
| variable     | cbTx           | C           | Required  | The fully |
|              |                | Transaction |           | s         |
|              |                |             |           | erialized |
|              |                |             |           | coinbase  |
|              |                |             |           | tr        |
|              |                |             |           | ansaction |
|              |                |             |           | of        |
|              |                |             |           | ``bl      |
|              |                |             |           | ockHash`` |
+--------------+----------------+-------------+-----------+-----------+
| 1-9          | d              | compactSize | Required  | Number of |
|              | eletedMNsCount | uint        |           | ProRegTx  |
|              |                |             |           | hashes    |
|              |                |             |           | which     |
|              |                |             |           | were      |
|              |                |             |           | deleted   |
|              |                |             |           | after     |
|              |                |             |           | base      |
|              |                |             |           | BlockHash |
+--------------+----------------+-------------+-----------+-----------+
| variable     | deletedMNs     | vector      | Required  | A list of |
|              |                |             |           | ProRegTx  |
|              |                |             |           | hashes    |
|              |                |             |           | for       |
|              |                |             |           | m         |
|              |                |             |           | asternode |
|              |                |             |           | which     |
|              |                |             |           | were      |
|              |                |             |           | deleted   |
|              |                |             |           | after     |
|              |                |             |           | ``baseBl  |
|              |                |             |           | ockHash`` |
+--------------+----------------+-------------+-----------+-----------+
| variable     | mnList         | vector      | Required  | The list  |
|              |                |             |           | of        |
|              |                |             |           | S         |
|              |                |             |           | implified |
|              |                |             |           | M         |
|              |                |             |           | asternode |
|              |                |             |           | List      |
|              |                |             |           | (SML)     |
|              |                |             |           | entries   |
|              |                |             |           | which     |
|              |                |             |           | were      |
|              |                |             |           | added or  |
|              |                |             |           | updated   |
|              |                |             |           | since     |
|              |                |             |           | ``baseBl  |
|              |                |             |           | ockHash`` |
+--------------+----------------+-------------+-----------+-----------+
| 1-9          | delet          | compactSize | Required  | *Added in |
|              | edQuorumsCount | uint        |           | protocol  |
|              |                |             |           | version   |
|              |                |             |           | 70214     |
|              |                |             |           | *\ Number |
|              |                |             |           | of LLMQs  |
|              |                |             |           | which     |
|              |                |             |           | were      |
|              |                |             |           | deleted   |
|              |                |             |           | from the  |
|              |                |             |           | active    |
|              |                |             |           | set after |
|              |                |             |           | ``baseBl  |
|              |                |             |           | ockHash`` |
+--------------+----------------+-------------+-----------+-----------+
| variable     | deletedQuorums | (uint8_t    | Required  | *Added in |
|              |                | +uint256)[] |           | protocol  |
|              |                |             |           | version   |
|              |                |             |           | 70214*\ A |
|              |                |             |           | list of   |
|              |                |             |           | LLMQ type |
|              |                |             |           | and       |
|              |                |             |           | quorum    |
|              |                |             |           | hashes    |
|              |                |             |           | for LLMQs |
|              |                |             |           | which     |
|              |                |             |           | were      |
|              |                |             |           | deleted   |
|              |                |             |           | after     |
|              |                |             |           | ``baseBl  |
|              |                |             |           | ockHash`` |
+--------------+----------------+-------------+-----------+-----------+
| 1-9          | n              | compactSize | Required  | *Added in |
|              | ewQuorumsCount | uint        |           | protocol  |
|              |                |             |           | version   |
|              |                |             |           | 70214     |
|              |                |             |           | *\ Number |
|              |                |             |           | of new    |
|              |                |             |           | LLMQs     |
|              |                |             |           | which     |
|              |                |             |           | were      |
|              |                |             |           | added to  |
|              |                |             |           | the       |
|              |                |             |           | active    |
|              |                |             |           | set since |
|              |                |             |           | ``baseBl  |
|              |                |             |           | ockHash`` |
+--------------+----------------+-------------+-----------+-----------+
| variable     | newQuorums     | qfcommit[]  | Required  | *Added in |
|              |                |             |           | protocol  |
|              |                |             |           | version   |
|              |                |             |           | 70        |
|              |                |             |           | 214*\ The |
|              |                |             |           | list of   |
|              |                |             |           | LLMQ      |
|              |                |             |           | co        |
|              |                |             |           | mmitments |
|              |                |             |           | for the   |
|              |                |             |           | LLMQs     |
|              |                |             |           | which     |
|              |                |             |           | were      |
|              |                |             |           | added     |
|              |                |             |           | since     |
|              |                |             |           | ``baseBl  |
|              |                |             |           | ockHash`` |
+--------------+----------------+-------------+-----------+-----------+

Simplified Masternode List (SML) Entry

+------------------+--------------------+--------------+--------------+
| Bytes            | Name               | Data type    | Description  |
+==================+====================+==============+==============+
| 32               | proRegTxHash       | uint256      | The hash of  |
|                  |                    |              | the ProRegTx |
|                  |                    |              | that         |
|                  |                    |              | identifies   |
|                  |                    |              | the          |
|                  |                    |              | masternode   |
+------------------+--------------------+--------------+--------------+
| 32               | confirmedHash      | uint256      | The hash of  |
|                  |                    |              | the block at |
|                  |                    |              | which the    |
|                  |                    |              | masternode   |
|                  |                    |              | got          |
|                  |                    |              | confirmed    |
+------------------+--------------------+--------------+--------------+
| 16               | ipAddress          | byte[]       | IPv6 address |
|                  |                    |              | in network   |
|                  |                    |              | byte order.  |
|                  |                    |              | Only IPv4    |
|                  |                    |              | mapped       |
|                  |                    |              | addresses    |
|                  |                    |              | are allowed  |
|                  |                    |              | (to be       |
|                  |                    |              | extended in  |
|                  |                    |              | the future)  |
+------------------+--------------------+--------------+--------------+
| 2                | port               | uint_16      | Port         |
|                  |                    |              | (network     |
|                  |                    |              | byte order)  |
+------------------+--------------------+--------------+--------------+
| 48               | pubKeyOperator     | BLSPubKey    | The          |
|                  |                    |              | operators    |
|                  |                    |              | public key   |
+------------------+--------------------+--------------+--------------+
| 20               | keyIDVoting        | CKeyID       | The public   |
|                  |                    |              | key hash     |
|                  |                    |              | used for     |
|                  |                    |              | voting.      |
+------------------+--------------------+--------------+--------------+
| 1                | isValid            | bool         | True if a    |
|                  |                    |              | masternode   |
|                  |                    |              | is not       |
|                  |                    |              | PoSe-banned  |
+------------------+--------------------+--------------+--------------+

The following annotated hexdump shows a ```mnlistdiff``
message <core-ref-p2p-network-data-messages#mnlistdiff>`__. (The message
header has been omitted.)

.. code:: text

   000001ee5108348a2c59396da29dc576
   9b2a9bb303d7577aee9cd95136c49b9b ........... Base block hash

   0000030f51f12e7069a7aa5f1bc9085d
   db3fe368976296fd3b6d73fdaf898cc0 ........... Block hash

   05000000 ................................... Transactions: 5

   04 ......................................... Merkle hash count: 4

   4488a599e5d61709664c32305befd58b
   ef29e33bc6e718af0233f938557a57a9 ........... Merkle hash 1
   5c8119b7b136d94e477a0d2917d5f724
   5ff299cc6e31994f6236a8fb34fec88f ........... Merkle hash 2
   905efa3e6743c889823f00147d36d12f
   d12ad401c19089f0affcabd423deef67 ........... Merkle hash 3
   3f3a7f84d7ad33214994b5aecf4c1e19
   2cb65b86750b1377e069073d1eba477a ........... Merkle hash 4

   01 ......................................... Merkle flag count: 1
   0f ......................................... Flags: 0 0 0 0 1 1 1 1

   [...]....................................... Coinbase Tx (Not shown)

   00 ......................................... Deleted masternodes: 0

   02 ......................................... Masternode list entries: 2

   00 ......................................... Deleted quorums: 0

   00 ......................................... New quorums: 0

   Masternode List
   | Masternode 1
   | | 01040eb32f760490054543356cff4638
   | | 65633439dd073cffa570305eb086f70e ....... ProRegTx hash
   | |
   | | 000001ee5108348a2c59396da29dc576
   | | 9b2a9bb303d7577aee9cd95136c49b9b ....... Confirmed block hash
   | |
   | | 00000000000000000000000000000000 ....... IP Address: ::ffff:0.0.0.0
   | | 0000 ................................... Port: 0
   | |
   | | 0000000000000000000000000000000000000000
   | | 0000000000000000000000000000000000000000
   | | 0000000000000000 ....................... Operator public key (BLS)
   | | c2ae01fb4084cbc3bc31e7f59b36be228a320404 Voting pubkey hash (ECDSA)
   | |
   | | 0 ...................................... Valid (0 - No)
   |
   | Masternode 2
   | | f7737beb39779971e9bc59632243e13f
   | | c5fc9ada93b69bf48c2d4c463296cd5a ....... ProRegTx hash
   | |
   | | 0000030f51f12e7069a7aa5f1bc9085d
   | | db3fe368976296fd3b6d73fdaf898cc0 ....... Confirmed block hash
   | |
   | | 000000000000000000000000cf9af40d ....... IP Address: ::ffff:207.154.244.13
   | | 4e1f ................................... Port: 19999
   | |
   | | 88d719278eef605d9c19037366910b59bc28d437
   | | de4a8db4d76fda6d6985dbdf10404fb9bb5cd0e8
   | | c22f4a914a6c5566 ....................... Operator public key (BLS)
   | | 43ce12751c4ba45dcdfe2c16cefd61461e17a54d Voting pubkey hash (ECDSA)
   | |
   | | 1 ...................................... Valid (1 - Yes)

notfound
========

*Added in protocol version 70001.*

The ```notfound``
message <core-ref-p2p-network-data-messages#notfound>`__ is a reply to a
```getdata`` message <core-ref-p2p-network-data-messages#getdata>`__
which requested an object the receiving <> does not have available for
relay. (Nodes are not expected to relay historic transactions which are
no longer in the memory pool or relay set. Nodes may also have pruned
spent transactions from older <>, making them unable to send those
blocks.)

The format and maximum size limitations of the ```notfound``
message <core-ref-p2p-network-data-messages#notfound>`__ are identical
to the ```inv`` message <core-ref-p2p-network-data-messages#inv>`__;
only the message header differs.

qrinfo
======

*Added in protocol version 70222 of Dash Core.*

The ``qrinfo`` message sends quorum rotation information to a node which
previously requested it with a ```getqrinfo``
message <core-ref-p2p-network-data-messages#getqrinfo>`__.

Note: In the following fields, ``c`` refers to the quorum cycle length.
This is synonymous with the DKG interval (``quorumDkgInterval``) as
defined in
`DIP6 <https://github.com/dashpay/dips/blob/master/dip-0006.md#parametersvariables-of-a-llmq-and-dkg>`__.

+--------------+----------------+-------------+-----------+-----------+
| Bytes        | Name           | Data type   | Required  | De        |
|              |                |             |           | scription |
+==============+================+=============+===========+===========+
| Varies       | quorumSna      | CQuo        | Required  | Quorum    |
|              | pshotAtHMinusC | rumSnapshot |           | snapshot  |
|              |                |             |           | for       |
|              |                |             |           | height    |
|              |                |             |           | ``h-c``   |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | quorumSnap     | CQuo        | Required  | Quorum    |
|              | shotAtHMinus2C | rumSnapshot |           | snapshot  |
|              |                |             |           | for       |
|              |                |             |           | height    |
|              |                |             |           | ``h-2c``  |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | quorumSnap     | CQuo        | Required  | Quorum    |
|              | shotAtHMinus3C | rumSnapshot |           | snapshot  |
|              |                |             |           | for       |
|              |                |             |           | height    |
|              |                |             |           | ``h-3c``  |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | mnListDiffTip  | CSi         | Required  | M         |
|              |                | mplifiedMNL |           | asternode |
|              |                | istDiff(see |           | list diff |
|              |                | ```mnlistdi |           | at height |
|              |                | ff`` <#mnli |           | at the    |
|              |                | stdiff>`__) |           | tip.      |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | mnListDiffH    | CSi         | Required  | M         |
|              |                | mplifiedMNL |           | asternode |
|              |                | istDiff(see |           | list diff |
|              |                | ```mnlistdi |           | at height |
|              |                | ff`` <#mnli |           | ``h``.    |
|              |                | stdiff>`__) |           |           |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | mnLis          | CSi         | Required  | M         |
|              | tDiffAtHMinusC | mplifiedMNL |           | asternode |
|              |                | istDiff(see |           | list diff |
|              |                | ```mnlistdi |           | at height |
|              |                | ff`` <#mnli |           | ``h-c``   |
|              |                | stdiff>`__) |           |           |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | mnList         | CSi         | Required  | M         |
|              | DiffAtHMinus2C | mplifiedMNL |           | asternode |
|              |                | istDiff(see |           | list diff |
|              |                | ```mnlistdi |           | at height |
|              |                | ff`` <#mnli |           | ``h-2c``  |
|              |                | stdiff>`__) |           |           |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | mnList         | CSi         | Required  | M         |
|              | DiffAtHMinus3C | mplifiedMNL |           | asternode |
|              |                | istDiff(see |           | list diff |
|              |                | ```mnlistdi |           | at height |
|              |                | ff`` <#mnli |           | ``h-3c``  |
|              |                | stdiff>`__) |           |           |
+--------------+----------------+-------------+-----------+-----------+
| 1            | extraShare     | bool        | Required  | Flag to   |
|              |                |             |           | indicate  |
|              |                |             |           | if an     |
|              |                |             |           | extra     |
|              |                |             |           | share is  |
|              |                |             |           | requested |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | quorumSnap     | CQuo        | Optional  | Returned  |
|              | shotAtHMinus4C | rumSnapshot |           | only if   |
|              |                |             |           | ``ext     |
|              |                |             |           | raShare`` |
|              |                |             |           | is on.    |
|              |                |             |           | See below |
|              |                |             |           | for       |
|              |                |             |           | su        |
|              |                |             |           | b-message |
|              |                |             |           | contents. |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | mnList         | CSimplifie  | Optional  | Returned  |
|              | DiffAtHMinus4C | dMNListDiff |           | only if   |
|              |                |             |           | ``ext     |
|              |                |             |           | raShare`` |
|              |                |             |           | is on. As |
|              |                |             |           | in DIP-4. |
+--------------+----------------+-------------+-----------+-----------+
| 1-9          | blo            | compactSize | Required  | Number of |
|              | ckHashListSize | uint        |           | elements  |
|              |                |             |           | in        |
|              |                |             |           | ``blockH  |
|              |                |             |           | ashList`` |
+--------------+----------------+-------------+-----------+-----------+
| 32 \*        | blockHashList  | uint256_t[] | Required  | Contains  |
| ``blo        |                |             |           | the last  |
| ckHash``\ \  |                |             |           | creation  |
| ``ListSize`` |                |             |           | block     |
|              |                |             |           | hash of   |
|              |                |             |           | each      |
|              |                |             |           | quo       |
|              |                |             |           | rumIndex. |
|              |                |             |           | Ordered   |
|              |                |             |           | by        |
|              |                |             |           | qu        |
|              |                |             |           | orumIndex |
+--------------+----------------+-------------+-----------+-----------+
| 1-9          | quorumSn       | compactSize | Required  | Number of |
|              | apshotListSize | uint        |           | elements  |
|              |                |             |           | in        |
|              |                |             |           | ``qu      |
|              |                |             |           | orumSnaps |
|              |                |             |           | hotList`` |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | quor           | CQuoru      | Required  | The       |
|              | umSnapshotList | mSnapshot[] |           | snapshots |
|              |                |             |           | required  |
|              |                |             |           | to        |
|              |                |             |           | re        |
|              |                |             |           | construct |
|              |                |             |           | the       |
|              |                |             |           | quorums   |
|              |                |             |           | built at  |
|              |                |             |           | ``h`` in  |
|              |                |             |           | heig      |
|              |                |             |           | htsLists. |
|              |                |             |           | Ordered   |
|              |                |             |           | from      |
|              |                |             |           | oldest to |
|              |                |             |           | newest    |
+--------------+----------------+-------------+-----------+-----------+
| 1-9          | mnLi           | compactSize | Required  | Number of |
|              | stDiffListSize | uint        |           | elements  |
|              |                |             |           | in        |
|              |                |             |           | ``mnListD |
|              |                |             |           | iffList`` |
+--------------+----------------+-------------+-----------+-----------+
| Varies       | mnListDiffList | C           | Required  | The       |
|              |                | SimplifiedM |           | MN        |
|              |                | NListDiff[] |           | LISTDIFFs |
|              |                |             |           | required  |
|              |                |             |           | to        |
|              |                |             |           | calculate |
|              |                |             |           | older     |
|              |                |             |           | quorums.  |
|              |                |             |           | Ordered   |
|              |                |             |           | from      |
|              |                |             |           | oldest to |
|              |                |             |           | newest    |
+--------------+----------------+-------------+-----------+-----------+

**CQuorumSnapshot**

Note: All fields are required

+-----------------+-------------------+----------------+--------------+
| Bytes           | Name              | Data type      | Description  |
+=================+===================+================+==============+
| 4               | mnSkipListMode    | int32_t        | Mode of the  |
|                 |                   |                | skip list    |
+-----------------+-------------------+----------------+--------------+
| 1-9             | active            | compactSize    | Number of    |
|                 | QuorumMembersSize | uint           | elements in  |
|                 |                   |                | ``activeQuo  |
|                 |                   |                | rumMembers`` |
+-----------------+-------------------+----------------+--------------+
| (``acti         | ac                | cbitset        | The bitset   |
| veQuorum``\ \ ` | tiveQuorumMembers |                | of nodes     |
| `MembersSize``) |                   |                | already in   |
| + 7)/8          |                   |                | quarters at  |
|                 |                   |                | the start of |
|                 |                   |                | cycle at     |
|                 |                   |                | height n     |
+-----------------+-------------------+----------------+--------------+
| 1-9             | mnSkipListSize    | compactSize    | Number of    |
|                 |                   | uint           | elements in  |
|                 |                   |                | mnSkipList   |
+-----------------+-------------------+----------------+--------------+
| 4 \*            | mnSkipList        | int32_t[]      | Skiplist at  |
| ``m             |                   |                | height n     |
| nSkipListSize`` |                   |                |              |
+-----------------+-------------------+----------------+--------------+

The following annotated hexdump shows a ```qrinfo``
message <core-ref-p2p-network-data-messages#qrinfo>`__. (The message
header has been omitted.)

.. code:: text

   Quorum snapshot (h-c)
   | 01000000 ................................. Skiplist mode: 1
   | 1f ....................................... Active quorum members: 31
   | bbffff7f ................................. Active quorum members bitset
   | 
   | 01 ....................................... Skip list size: 1
   | 
   | Skip list
   | | 0b000000 ............................... Masternode 11

   Quorum snapshot (h-2c)
   | 01000000 ................................. Skiplist mode: 1
   | 1f ....................................... Active quorum members: 31
   | f3ff6f7b ................................. Active quorum members bitset
   |
   | 06 ....................................... Skip list size: 6
   | 
   | Skip list
   | | 05000000 ............................... Masternode 5
   | | 03000000 ............................... Masternode 3
   | | 04000000 ............................... Masternode 4
   | | 06000000 ............................... Masternode 6
   | | 07000000 ............................... Masternode 7
   | | 08000000 ............................... Masternode 8

   Quorum snapshot (h-3c) ..................... Not shown for brevity

   MnListDiff (Tip)
   | e173b01943fe7b8d2bf5a13c034eafed
   | d2708a1a0f9e5104b86439382e050000 ......... Base block hash
   | 847b42ae4509c82e8c8ba599f23f15c1
   | e34c899a09e4ebf440f6e2ef4b000000 ......... Block hash
   | 01000000 ................................. Transactions: 1
   | 01 ....................................... Merkle hash count: 1
   | c44c59fd87c816506d3890aaf0c8f7df
   | bad10f4a9b3e2b43bc1e9c832e81b381 ......... Merkle hash 1
   | 01 ....................................... Merkle flag count: 1
   | 01 ....................................... Flags: 0 0 0 0 0 0 0 1
   |
   | [...]..................................... Coinbase Tx (Not shown)

   MnListDiff (h) ............................. Not shown for brevity
   MnListDiff (h-c) ........................... Not shown for brevity
   MnListDiff (h-2c) .......................... Not shown for brevity
   MnListDiff (h-3c) .......................... Not shown for brevity

   01 ....................................... Extra share: true

   Quorum snapshot (h-4c)
   | 01000000 ................................. Skiplist mode: 1
   | 1f ....................................... Active quorum members: 31
   | ff7fdd5f ................................. Active quorum members bitset
   |
   | 01 ....................................... Skip list size: 1
   |
   | Skip list
   | | 07000000 ............................... Masternode 7

   MnListDiff (h-4c)

   Block hash list
   | 04 ....................................... Block hashes: 4
   | c2e276c518b3f8484b237e300094e222
   | 388a095f486c822585a67b1520010000 ......... Block hash 1
   | bb4f0c5a3efb229bd5a4e4e45ca2228e
   | b11f05c3a6e8fdf9d7d4f499b7010000 ......... Block hash 2
   | aca22eb2c63daa5304cb9cbe6089afe3
   | 99e335f73306735718355ad3d9000000 ......... Block hash 3
   | 45bf29bada522857d6e3b2371e1fefc7
   | c01fcef0b6578e7062fd571760010000 ......... Block hash 4

   00 ......................................... Quorum snapshot list size: 0
   00 ......................................... Masternode list diff list size: 0

tx
==

The ```tx`` message <core-ref-p2p-network-data-messages#tx>`__ transmits
a single transaction in the <> format. It can be sent in a variety of
situations;

-  **Transaction Response:** Dash Core will send it in response to a
   ```getdata`` message <core-ref-p2p-network-data-messages#getdata>`__
   that requests the transaction with an <> type of ``MSG_TX``.

-  **MerkleBlock Response:** Dash Core will send it in response to a
   ```getdata`` message <core-ref-p2p-network-data-messages#getdata>`__
   that requests a <> with an <> type of ``MSG_MERKLEBLOCK``. (This is
   in addition to sending a ```merkleblock``
   message <core-ref-p2p-network-data-messages#merkleblock>`__.) Each
   ```tx`` message <core-ref-p2p-network-data-messages#tx>`__ in this
   case provides a matched transaction from that <>.

For an example hexdump of the raw transaction format, see the `raw
transaction section <core-ref-transactions-raw-transaction-format>`__.
