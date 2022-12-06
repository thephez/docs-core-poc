This section describes the Dash P2P network protocol (but it is not a
specification). It does not describe the <>, the `GetBlockTemplate
mining
protocol <core-guide-mining-block-prototypes#getblocktemplate-rpc>`__,
or any network protocol never implemented in an official version of Dash
Core.

All peer-to-peer communication occurs entirely over TCP. [block:callout]
{ “type”: “warning”, “body”: “Note: unless their description says
otherwise, all multi-byte integers mentioned in this section are
transmitted in little-endian order.” } [/block]

[block:image] { “images”: [ { “image”: [
“https://files.readme.io/2f6f207-home-map-1.svg”, “home-map-1.svg”, 259,
150, “#000000” ], “sizing”: “55” } ] } [/block]
