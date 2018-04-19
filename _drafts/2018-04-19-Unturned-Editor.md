---
layout: post
title: Unturned Editor
date: 2014-08-02 10:10:00
tags: c++ qt
---

**Outdated**

Unturned is a sandbox multiplayer game much like Minecraft. It was recently Greenlit on Steam, and gained a huge amount of popularity in the following few days. The game currently saves player data in the registry (on Windows). Which I thought was a bit strange. Turns out, the developer used PlayerPrefs from Unity.

The key value needs to be converted into Windows-1252 encoded bytes, then decoded as if it were encoded in UTF-8. Each character’s unicode will then need to have ’32’ subtracted from it. Then we mod every character by 255, such that we get an ASCII character. You can read more about the encoding/decoding process here.

On Macs, save data is actually stored in a .plist file. Which can be parsed, and only need to be decoded as if it were UTF-8 encoded. Here is a prototype application which parses the .plist file and displays the decoded ‘inventory_’ value. We can then edit this string to give the player any item, ammo, etc.
