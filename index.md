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
	
	Categories have been condensed to "client," "server," "tools" and "other" until we get more projects added. I'm 
	leaving out the "client" category until we actually get a client project on the list. Also, whenever we get more
	projects it may be beneficial to have some form of navigation in order to navigate quickly to the category you're
	looking for.
-->

{% include project-category-header.html name="Server" description="A server project can be anything from a wrapper to a complete recode of the official server." %}
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
		<tr> <!-- MultiBound -->
            <td><a target="_blank" href="https://github.com/ROFLCopter64bit/MultiBound">ROFLCopter64bit/MultiBound</a></td>
            <td>Full recode of a server.</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>C#</td>
            <td><a target="_blank" href="https://github.com/ROFLCopter64bit/MultiBound/blob/master/LICENSE">GPL v2</a></td>
        </tr>
        <tr> <!-- StarryboundServer -->
            <td><a target="_blank" href="https://github.com/AvilanceLtd/StarryboundServer/">AvilanceLtd/StarryboundServer</a></td>
            <td>Starrybound Proxy Wrapper for Starbound Servers.</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>C#</td>
            <td><a target="_blank" href="https://github.com/AvilanceLtd/StarryboundServer/blob/master/LICENSE">GPLv3</a></td>
        </tr>
        <tr> <!-- StarryPy -->
            <td><a target="_blank" href="https://github.com/CarrotsAreMediocre/StarryPy">CarrotsAreMediocre/StarryPy</a></td>
            <td>Plugin-driven Starbound proxy server built using Twisted.</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-success">Stable</span></td>
            <td>Python</td>
            <td><a target="_blank" href="https://github.com/CarrotsAreMediocre/StarryPy/blob/master/LICENSE">WTFPL</a></td>
        </tr>
        <tr> <!-- SharpStar -->
            <td><a target="_blank" href="https://github.com/Mitch528/SharpStar">Mitch528/SharpStar</a></td>
            <td>Proxy server with support for C#, Python, Javascript, and Lua plugins</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-success">Stable</span></td>
            <td>C#</td>
            <td><a target="_blank" href="https://github.com/Mitch528/SharpStar/blob/master/LICENSE">GPLv3</a></td>
        </tr>
        <tr> <!-- StarNub -->
            <td><a target="_blank" href="https://github.com/StarNub/StarNub">StarNub/StarNub</a></td>
            <td>Proxy server with plugin framework & Starbound server managment.</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-success">Stable</span></td>
            <td>Java</td>
            <td><a target="_blank" href="https://github.com/StarNub/StarNub/blob/stable/LICENSE">MIT</a></td>
        </tr>
	</tbody>
</table>

{% include project-category-header.html name="Tools" description="Tools are either a library or a program that can be used to view and/or edit some form of data (player, world, etc.) file(s)." %}
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
		<tr> <!-- py-starbound -->
            <td><a target="_blank" href="https://github.com/blixt/py-starbound">blixt/py-starbound</a></td>
            <td>Parser for Starbound's file formats (.pak, .player, .world, …)</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-success">Stable</span></td>
            <td>Python</td>
            <td><a target="_blank" href="https://github.com/blixt/py-starbound/blob/master/LICENSE">MIT</a></td>
        </tr>
        <tr> <!-- starbounded -->
            <td><a target="_blank" href="https://github.com/blixt/starbounded">blixt/StarboundEd</a></td>
            <td>Web app for loading and display worlds</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>JavaScript</td>
            <td><a target="_blank" href="https://github.com/blixt/starbounded/blob/master/LICENSE">MIT</a></td>
        </tr>
        <tr> <!-- starbound-research -->
            <td><a target="_blank" href="https://github.com/McSimp/starbound-research">McSimp/starbound-research</a></td>
            <td>Scripts for reading various starbound data files in 010 Editor</td>
            <td><span class="label label-danger">Furious Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>010 Editor</td>
            <td><a target="_blank" href="https://github.com/McSimp/starbound-research/blob/master/LICENSE">MIT</a></td>
        </tr>
        <tr> <!-- StarDB -->
            <td><a target="_blank" href="https://github.com/McSimp/StarDB">McSimp/StarDB</a></td>
            <td>A Python library for manipulating Starbound database files.</td>
            <td><span class="label label-danger">Furious Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>Python</td>
            <td><a target="_blank" href="https://github.com/McSimp/StarDB/blob/master/LICENSE">MIT</a></td>
        </tr>
        <tr> <!-- StarDB-for-Java -->
            <td><a target="_blank" href="https://github.com/KrazyTheFox/StarDB-for-Java">KrazyTheFox/StarDB-for-Java</a></td>
            <td>A Java port of StarDB.</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-success">Stable</span></td>
            <td>Java</td>
            <td><a target="_blank" href="https://github.com/KrazyTheFox/StarDB-for-Java/blob/master/LICENSE">MIT</a></td>
        </tr>
        <tr> <!-- starcheat -->
            <td><a target="_blank" href="https://github.com/wizzomafizzo/starcheat">wizzomafizzo/starcheat</a></td>
            <td>Python modules for parsing/editing .player files and assets</td>
            <td><span class="label label-success">Enraged Koala</span></td>
            <td><span class="label label-success">Stable</span></td>
            <td>Python</td>
            <td><a target="_blank" href="https://github.com/wizzomafizzo/starcheat/blob/master/LICENSE">MIT</a></td>
        </tr>
	</tbody>
</table>

{% include project-category-header.html name="Other" description="Projects in this category either don't fit under any of the categories above or the project submitter was unsure what category to put the project under." %}
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
        <tr> <!-- StarNet -->
			<!-- I was unsure about where to put this project. -->
            <td><a target="_blank" href="https://github.com/SirCmpwn/StarNet">SirCmpwn/StarNet</a></td>
            <td>Starbound multiplayer network infrastructure.</td>
            <td><span class="label label-danger">Furious Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>C#</td>
            <td><a target="_blank" href="https://github.com/SirCmpwn/StarNet/blob/master/LICENSE">MIT</a></td>
        </tr>
		<tr> <!-- Pixxel -->
            <td><a target="_blank" href="https://github.com/LostGamesStudio/Pixxel">LostGamesStudio/Pixxel</a></td>
            <td>Starbound server wrapper being developed by AegisGaming and LostGamesStudio</td>
            <td><span class="label label-success">Furious Koala</span></td>
            <td><span class="label label-danger">Unstable</span></td>
            <td>Java</td>
            <td><a target="_blank" href="https://github.com/LostGamesStudio/Pixxel/blob/master/README.md">None</a></td>
        </tr>
    </tbody>
</table>
