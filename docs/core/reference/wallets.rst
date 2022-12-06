Wallets
*******

Deterministic Wallet Formats
============================

Type 1: Single Chain Wallets
----------------------------

A Type 1 deterministic <> is the simpler of the two, which can create a
single series of keys from a single seed. A primary weakness is that if
the seed is leaked, all funds are compromised, and wallet sharing is
extremely limited.

Type 2: Hierarchical Deterministic (HD) Wallets
-----------------------------------------------

.. figure:: https://dash-docs.github.io/img/dev/en-hd-overview.svg
   :alt: Overview Of Hierarchical Deterministic Key Derivation

   Overview Of Hierarchical Deterministic Key Derivation

For an overview of the <>, please see the `developer guide
section <core-guide-wallets>`__. For details, please see
`BIP32 <https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki>`__.
