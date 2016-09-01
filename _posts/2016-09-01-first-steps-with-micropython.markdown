---
layout: post
title: "First steps with Micropython on a NodeMCU"
permalink: /micropython
date: 2016-09-01
comments: false
---
<div style="float: right" markdown="1">
![micropython](/assets/micropython.jpg)
</div>

My NodeMCU arrived so I went right ahead and installed Micropython on it.

Install the Micropython firmware
================================

The esptool tool is used to copy the firmware onto the board. It can be
installed with pip. I created a virtualenv for that. esptool requires python2
so the virtualenv was configured to use that.

```shell
$ mkvirtualenv -p /usr/bin/python2.7 esptool && setvirtualenvproject
```

Install esptool in the virtualenv:

```
$ pip install esptool
```

Downloaded the [pre-built micropython firmware](http://micropython.org/download/)
to put onto the board.

Plugging in the nodemcu gives me `/dev/ttyUSB0`.

Erase any existing firmware:

```
$ esptool.py --port /dev/ttyUSB0 erase_flash
```

Write the micropython firmware:

```
$ esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=8m -fm dio 0 esp8266-20160809-v1.8.3.bin
```

**Now replug the device.**

Enter the REPL
==============

I used picocom.

```shell
$ picocom -b115200 -ep /dev/ttyUSB0
```
```
picocom v2.1

port is        : /dev/ttyUSB0
flowcontrol    : none
baudrate is    : 115200
parity is      : none
databits are   : 8
stopbits are   : 1
escape is      : C-p
local echo is  : no
noinit is      : no
noreset is     : no
nolock is      : no
send_cmd is    : sz -vv
receive_cmd is : rz -vv -E
imap is        :
omap is        :
emap is        : crcrlf,delbs,

Type [C-p] [C-h] to see available commands

Terminal ready
```

Press enter to see the prompt:

```
>>>
```

Notes
=====

- **Replug the device** after copying the firmware over.

- **Specify the baudrate** of 115200. Without this it said *Terminal Ready*,
but there was no prompt and I couldn't communicate at all. Once I specified the
baudrate with the `-b` option, the prompt appears (after pressing enter).

- I changed picocom's escape command to `C-p` with the `-e` option, because
`C-a` clashed with my tmux setup.

- To exit use `[C-p]`, ``C-\``, `[C-p]`, `C-x`.