---
title: Data Types
layout: base
---

# Data Types

Starbound uses several different data types to move information over the wire. These types are used on the [protocol](/networking) documentation.

* **string**: A length prefixed UTF-8 string, with a VLQ specifying the length.
* **(u)int##**: An integer, in big-endian order. "u" indicates unsiged and ## is the length, in bits. Examples: uint8, int32, etc.
* **(s)VLQ**: [Variable length quantity](https://en.wikipedia.org/wiki/Variable-length_quantity). "s" indicates signed. See below.
* **(u)int##[]**: An integer array. Arrays without a fixed length are prefixed with a VLQ indicating their length. Examples: uint8[], int32[10]
* **bool**: A boolean value (true or false). Encoded as a uint8, 0 indicates false and 1 indicates true.
* **float**: 32-bit IEEE 754 floating point number.
* **double**: 64-bit IEEE 754 floating point number.
* **Variant**: Data serialization format. See below.

## Implementations

* C# [All data types](https://github.com/SirCmpwn/StarNet/blob/master/StarNet/StarboundStream.cs) ([MIT license](https://github.com/SirCmpwn/StarNet/blob/master/LICENSE))

## VLQ

Variable length quantities (VLQs) are widely used throughout starbound. An example by conversion follows:
To convert 601000 to an unsigned VLQ, starting in little-endian, we end up with a binary representation of 601000 as `10010010101110101000`.

The next step is to split the binary into groups of 7, starting from the LSB. This gives us `100100 1010111 0101000`. We then pad all but the LSB segment with 1, to signify it's a continuation bit. On the MSB, we pad as necessary with 0s to achieve a full octet.

The result from this operation is `10100100 11010111 00101000`, which is the unsigned VLQ. The conversion backwards is much the same.

A signed VLQ is very similar, but with a simple flag to demonstrate parity. First multiply the input value by two, and subtract one. Apply the VLQ algorithm to that output, and you're done. Easy as pie!

## Variant

The Variant type is a relatively complex structure that incorporates many other types into its construction.

The first byte of a variant indicates its type. Depending on that byte, one of 8 different formats may follow.

<table class="table">
    <thead>
        <tr>
            <th>Type</th>
            <th>Value</th>
            <th>Comments</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>null</td>
            <td></td>
        </tr>
        <tr>
            <td>2</td>
            <td>double</td>
            <td></td>
        </tr>
        <tr>
            <td>3</td>
            <td>bool</td>
            <td>1: true; 0: false</td>
        </tr>
        <tr>
            <td>4</td>
            <td>VLQ</td>
            <td></td>
        </tr>
        <tr>
            <td>5</td>
            <td>string</td>
            <td></td>
        </tr>
        <tr>
            <td>6</td>
            <td>Variant[]</td>
            <td>Prefixed with VLQ with item count</td>
        </tr>
        <tr>
            <td>7</td>
            <td>Dictionary</td>
            <td>Prefixed with VLQ indicating number of string/Variant key-value pairs</td>
        </tr>
    </tbody>
</table>
