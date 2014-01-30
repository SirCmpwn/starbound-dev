---
layout: base
title: Starbound Networking
---
{% capture content %}
# Starbound Networking

The Starbound networking protocol is based on TCP, and uses port 21025 by default. The current protocol version (Furious Koala) is 636.

## Resources

There are several related pages that you might want to browse:

* [Data types](/networking/data-types.html)
* [Logging on to a server](/networking/connecting.html)

## Base Packet

Each packet is wrapped in this basic package:

<table class="table table-bordered">
    <thead>
        <tr>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>uint8</td>
            <td>Packet ID</td>
            <td>The packet identifier. The payload format changes based on this value.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Payload length</td>
            <td>The length of the payload, in bytes. If it is negative, this indicates the payload is compressed using zlib.</td>
        </tr>
        <tr>
            <td>Varies</td>
            <td>Payload</td>
            <td>Varies</td>
        </tr>
    </tbody>
</table>

Though the server and client must be able to handle compressed packets, no packet *must* be compressed. Packets are compressed in the official client and server when it finds that the compressed payload is smaller than the uncompress payload.

## Packets

{% include packet-header.html name="Protocol Version" id="0" direction="server-to-client" %}

This packet is the first packet sent. It contains the server version.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">0</td></tr>
        <tr>
            <td>VLQ</td>
            <td>Protocol version number</td>
            <td>The server's supported protocol version. Changes with each release.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Connection Response" id="1" direction="server-to-client" %}

This packet tells the client whether their connection attempt is successful or if they have been rejected. It is the final packet sent in the handshake process.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">1</td></tr>
        <tr>
            <td>bool</td>
            <td>Success</td>
            <td>Whether or not the connection was successful.</td>
        </tr>
        <tr>
            <td>VLQ</td>
            <td>Client ID</td>
            <td>The identifier for the connecting client.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Rejection reason</td>
            <td>A string signifying why the user was rejected. This will be present even if the connection was successful, but will simply have a length prefix of 0.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Server Disconnect" id="2" direction="server-to-client" %}

This packet is used to notify the client of a disconnect.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">2</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Handshake Challenge" id="3" direction="server-to-client" %}

This packet provides a salt and round count for password verification. It is followed by [Handshake Response](#handshake-response).

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">3</td></tr>
        <tr>
            <td>string</td>
            <td>Claim Message</td>
            <td></td>
        </tr>
        <tr>
            <td>string</td>
            <td>Salt</td>
            <td>The salt to use when hashing the password.</td>
        </tr>
        <tr>
            <td>int32</td>
            <td>Round count</td>
            <td>The number of hashing rounds to perform on the password. Default: 5000</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Chat Recieved" id="4" direction="server-to-client" %}

This packet is sent to a client with a chat message.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="6">4</td></tr>
        <tr>
            <td>uint8</td>
            <td>Channel</td>
            <td>The chat channel.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>World</td>
            <td>The world the sender is on.</td>
        </tr>
        <tr>
            <td>uint32</td>
            <td>Client ID</td>
            <td>The sender's client ID.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Name</td>
            <td>The sender's name in game.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Name</td>
            <td>The chat message.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Universe Time Update" id="5" direction="server-to-client" %}

This packet is sent from the server to update the current time.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">5</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Client Connect" id="6" direction="client-to-server" %}

This packet is sent in the handshake process immediately after the [Protocol Version](#protocol-version). It contains all relevant data about the connecting player.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="9">6</td></tr>
        <tr>
            <td>uint8[]</td>
            <td>Asset Digest</td>
            <td>The digest of the asset folder. Used to detect if any mods are active.</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Claim</td>
            <td>Unknown, presumably relates to key exchange. Seems to be unused in the current version.</td>
        </tr>
        <tr>
            <td>bool</td>
            <td>UUID flag</td>
            <td>If true, a UUID follows. If false, it does not.</td>
        </tr>
        <tr>
            <td>uint8[16]</td>
            <td>UUID</td>
            <td>The UUID is read sequentially as the hex dump of the bytes read. This only exists if the UUID flag is true.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Player name</td>
            <td>The name of the player.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Species</td>
            <td>The species of the player.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>Shipworld</td>
            <td>The player's .shipworld file, without the file header information.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Account</td>
            <td>The player's account name, from the connection dialog.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Client Disconnect" id="7" direction="client-to-server" %}

This packet is sent when the client disconnects.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">7</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Handshake Response" id="8" direction="client-to-server" %}

This packet is the response to the [Handshake Challenge](#handshake-challenge).

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="3">8</td></tr>
        <tr>
            <td>string</td>
            <td>Claim Response</td>
            <td>A response to the claim message. Currently unused.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Password hash</td>
            <td>The hash of the entered password, salted with the information in <a href="#handshake-challenge">Handshake Challenge</a>.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Warp Command" id="9" direction="bidirectional" %}

This packet is sent when the player warps/is warped to a planet or ship.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">9</td></tr>
        <tr>
            <td>uint32</td>
            <td>Warp type</td>
            <td>The type of warp being done.</td>
        </tr>
        <tr>
            <td>world_coordinate</td>
            <td>World coordinates</td>
            <td>The world coordinates being warped to. Currently only used in Warp to Spawn.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Player name</td>
            <td>The name of the player being warped to. Currently only used to beam to other ships.</td>
        </tr>
    </tbody>
</table>

#### Warp Types

<table class="table">
    <thead>
        <tr>
            <th>Value</th>
            <th>Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>Move ship</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Warp to own ship</td>
        </tr>
        <tr>
            <td>3</td>
            <td>Warp to player ship</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Warp to orbited planet</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Warp to home planet</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Chat Sent" id="10" direction="client-to-server" %}

This packet is sent from the client whenever a message is sent in the chat window.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="3">10</td></tr>
        <tr>
            <td>string</td>
            <td>Message</td>
            <td>The chat message being sent.</td>
        </tr>
        <tr>
            <td>unit8</td>
            <td>Channel</td>
            <td>The current chat channel.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Client Context Update" id="11" direction="bidirectional" %}

This packet has yet to be fully understood.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">11</td></tr>
        <tr>
            <td>uint8[]</td>
            <td>Client Context Data</td>
            <td>Myriad formats.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="World Start" id="12" direction="server-to-client" %}

**Documentation for this packet is outdated.**

This packet is sent to the client when a world thread has been started on the server.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="10">12</td></tr>
        <tr>
            <td>uint8[]</td>
            <td>Planet</td>
            <td>The world data. WRLDB format with headers stripped.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>World structure</td>
            <td>The world structure. TODO.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>Sky</td>
            <td>Data relating to the sky. TODO.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>Server Weather</td>
            <td>Data relating to the server weather. TODO.</td>
        </tr>
        <tr>
            <td>float</td>
            <td>Spawn X</td>
            <td>The spawn coordinates for the planet. Currently locked in server-side.</td>
        </tr>
        <tr>
            <td>float</td>
            <td>Spawn Y</td>
            <td>The spawn coordinates for the planet. Currently locked in server-side.</td>
        </tr>
        <tr>
            <td>An Update World Properties packet.<br>(A variant dictionary but without the leading variant type)</td>
            <td>World Properties</td>
            <td>A dictionary with multiple key value pairs about world properties. See <a href="#0x2F">Update World Properties</a>.</td>
        </tr>
        <tr>
            <td>int32</td>
            <td>Unknown</td>
            <td>This field serves an unknown purpose. Only value of 1 has been observed.</td>
        </tr>
        <tr>
            <td>bool</td>
            <td>Unknown</td>
            <td>This field serves an unknown purpose. Only value of false has been observed.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="World Stop" id="13" direction="server-to-client" %}

Called when a world thread is stopped.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">13</td></tr>
        <tr>
            <td>string</td>
            <td>Status</td>
            <td>The status of the world.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Array Update" id="14" direction="server-to-client"%}

Called when an array of tiles has its properties updated.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">14</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Update" id="15" direction="server-to-client" %}

This packet is called when a tile is updated.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">15</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Liquid Update" id="16" direction="server-to-client" %}

This packet is sent when the liquid on a tile has changed position.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">16</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Damage Update" id="17" direction="server-to-client" %}

This packet is sent when a tile is damaged.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">17</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Modification Failure" id="18" direction="server-to-client" %}

This packet is sent when a packet cannot successfully be modified.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">18</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Give Item" id="19" direction="server-to-client" %}

This packet attempts to give an item to a player. If the player's inventory is full, it will drop on the ground next to them.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">19</td></tr>
        <tr>
            <td>string</td>
            <td>Item name</td>
            <td>The name of the item, from the ingame assets. Case sensitive.</td>
        </tr>
        <tr>
            <td>VLQ</td>
            <td>Count</td>
            <td>The number of items to give to the player. If greater than stackSize, it will give up to that amount.</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Item properties</td>
            <td>A variant giving the item properties; for non-randomly generated/non-modified items it is just the String variant type.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Swap in Container Result" id="20" direction="server-to-client" %}

This packet is sent whenever two items are swapped in an open container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">20</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Enviornment Update" id="21" direction="server-to-client" direction="server-to-client" %}

This packet is sent on an environment update.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">21</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Entity Interact Result" id="22" direction="server-to-client" %}

This packet contains the results of an entity interaction.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">22</td></tr>
        <tr>
            <td>uint8</td>
            <td>Client ID</td>
            <td>The client attempting to interact.</td>
        </tr>
        <tr>
            <td>int8</td>
            <td>Enitity ID</td>
            <td>The ID of the entity that client is trying to interact with.</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Results</td>
            <td>A variant describing the results of the interaction.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Modify Tile List" id="23" direction="client-to-server" %}

This packet contains a list of tiles and modifications to them.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">23</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Damage Tile" id="24" direction="client-to-server" %}

This packet updates a tile's damage

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">24</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Damage Tile Group" id="25" direction="client-to-server" %}

This packet updates an entire tile group's damage.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">25</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Request Drop" id="26" direction="client-to-server" %}

This packet requests an item drop (from a player's inventory?)

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">26</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Spawn Entity" id="27" direction="client-to-server" %}

This packet requests that the server spawn an entity.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">27</td><td colspan=3 align='center'>TODO</td></tr>
</table>

{% include packet-header.html name="Entity Interact" id="28" direction="client-to-server" %}

This packet is sent when a client attempts to interact with an entity.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">28</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Connect Wire" id="29" %}

This packet connects a wire.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">29</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Disconnect All Wires" id="30" %}

This packet disconnects all wires.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">30</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Open Container" id="31" direction="client-to-server" %}

This packet opens a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">31</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Close Container" id="32" direction="client-to-server" %}

This packet closes a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">32</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Swap in Container" id="33" direction="client-to-server" %}

This packet swaps an item in a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">33</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Item Apply in Container" id="34" direction="client-to-server" %}

This packet applies an item to another item in a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">34</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Start Crafting in Container" id="35" direction="client-to-server" %}

This packet initiates crafting on an item in a container (Used in pixel compressors and the like?)

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">35</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Stop Crafting in Container" id="36" direction="client-to-server" %}

This packet stops crafting on an item in a container

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">36</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Burn Container" id="37" direction="client-to-server" %}

This packet burns a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">37</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Clear Container" id="38" direction="client-to-server" %}

This packet clears a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">38</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="World Update" id="39" direction="client-to-server" %}

This packet contains a world update

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">39</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Entity Create" id="40" direction="bidirectional" %}

This packet creates an entity.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">40</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Entity Update" id="41" direction="bidirectional" %}

This packet updates an entity's properties.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">41</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Entity Destroy" id="42" direction="server-to-client" %}

This packet destroys an entity.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">42</td><td colspan=3 align='center'>TODO</td></tr>

    </tbody>
</table>

{% include packet-header.html name="Damage Notification" id="43" direction="bidirectional" %}

This packet notifies the receiver of damage received.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">43</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Status Effect Request" id="44" direction="client-to-server" %}

This packet requests a status effect from the server.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">44</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Update World Properties" id="45" direction="server-to-client" %}

This packet updates world properties.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">45</td></tr>
        <tr>
            <td>uint8</td>
            <td>Number of pairs</td>
            <td>(might be) The number of names/value pairs in this packet.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Property name</td>
            <td>The name of the property that is about to be set. Example: fuel.level</td>
        </tr>
        <tr>
            <td>Variant Type 4 (sVLQ)</td>
            <td>Property value</td>
            <td>The new value of the before mentioned property. Example: 800 </td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Heartbeat" id="46" direction="bidirectional" %}

This packet is periodically sent to inform the other party that the other end is still connected.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">46</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>
{% endcapture %}

<!--
    This stuff just defines the layout for the rest. We have to put it down here because
    jekyll can be a bit of a pain sometimes.
-->
<div class="row">
    <div class="col-md-3">
        <ul class="side-nav nav">
            <h4>Packets</h4>
            {{ packets }}
        </ul>
    </div>
    <div class="col-md-9">
        {{ content | markdownify }}
    </div>
</div>
