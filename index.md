---
layout: base
title: Starbound Reverse Engineering
---

Welcome to the Starbound reverse engineering hub! This is a community-maintained, unofficial website to collect information
about reverse engineering [Starbound](http://playstarbound.com). This website is open-source - you can help us reverse engineer
the game and offer your discoveries by submitting a pull request to [SirCmpwn/starbound-dev](https://github.com/SirCmpwn/starbound-dev).

Have you got questions, or do you want to get help with Starbound dev? Join our IRC channel,
<a target="_blank" href="http://webchat.freenode.net/?channels=##starbound-dev">##starbound-dev on irc.freenode.net</a>.

Though some of the information here may be valuable to modders, this website is **not** meant to document information for modding
the game. It is instead meant for reverse engineering the game and creating entirely new software based on that research.

## Projects

Want your project listed? Submit a pull request! Projects here should have some degree of usability, though they may not
yet be complete.

<!-- 
    These lists are for open-source projects only.
    Maintain these lists first by supported Starbound version, and second alphabetically. Don't compete for room here.
    External links should have target="_blank"
    Add categories when appropriate. I expect there to be a "server" and "client" section eventually, and probably a
    "libraries" section. Make an "other" section if your stuff doesn't fit into the existing categories.
-->

### Proxies

<table class="table">
    <thead>
        <tr>
            <th>Project</th>
            <th>Description</th>
            <th>Starbound</th>
            <th>Stability</th>
            <th>Languages</th>
            <th>License</th>
        </tr>
    </thead>
    <tbody>
        <tr> <!-- StarryPy -->
            <td><a target="_blank" href="https://github.com/CarrotsAreMediocre/StarryPy">CarrotsAreMediocre/StarryPy</a></td>
            <td>Plugin-driven Starbound proxy server built using Twisted.</td>
            <td><span class="label label-success">Furious Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>Python</td>
            <td><a target="_blank" href="https://github.com/CarrotsAreMediocre/StarryPy/blob/master/LICENSE">WTFPL</a></td>
        </tr>
        <tr> <!-- StarryboundServer -->
            <td><a target="_blank" href="https://github.com/AvilanceLtd/StarryboundServer/">AvilanceLtd/StarryboundServer</a></td>
            <td>Starrybound Proxy Wrapper for Starbound Servers.</td>
            <td><span class="label label-danger">Angry Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>C#</td>
            <td><a target="_blank" href="https://github.com/AvilanceLtd/StarryboundServer/blob/master/LICENSE">GPLv3</a></td>
        </tr>
    </tbody>
</table>
