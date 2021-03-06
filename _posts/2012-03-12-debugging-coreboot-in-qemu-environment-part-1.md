---
ID: 62707
post_title: 'Debugging coreboot in qemu environment &#8211; part 1'
author: Piotr Król
post_excerpt: ""
layout: post
permalink: >
  https://3mdeb.com/firmware/debugging-coreboot-in-qemu-environment-part-1/
published: true
post_date: 2012-03-12 21:30:00
tags:
  - coreboot
  - qemu
categories:
  - Firmware
---
First of all I use testing version of Debian - wheezy. Clone coreboot repository:  
```
git clone http://review.coreboot.org/p/coreboot
cd coreboot; make menuconfig
```

Configure FILO as apayload and use its latest version:
```
Payload -&gt; Add a payload -&gt; FILO Payload -&gt; FILO version -&gt; HEAD
```
Add verbose debugging messages:
```
Debugging -&gt; Check PIRQ table consistency Debugging -&gt; Output verbose malloc debug messages
Debugging -&gt; Output verbose ACPI debug messages Debugging -&gt; Enable debug messages for option ROM execution
Debugging -&gt; Built-in low-level shell Debugging -&gt; Trace function calls
```

Try to build:
```
make
```

If everything builds correctly you can process. Sometimes there is need to use 
cross compiler. To build one:  
```
cd util/crossgcc
./buildgcc
```

To explore `coreboot` code effectively I suggest to create tags and cscope 
database for `coreboot`. In my personal workspace I've got process that I go 
through before I start work (if you use my workspace configuration which 
is available [@github](https://github.com/pietrushnic/workspace) you 
can follow below steps directly, if not adjust to your environment):

1. run vim ;)
2. `:cd /path/to/code`
3. `s<Tab>` (fuzzyfinder -> bookmark dir)
4. `si` (fuzzyfinder -> change dir)
5. `sr` (run ctags to generate tags and cscope to build symbol database - `ctags -R;cscope -R -q -b -v`)

After steps above we can start work with code. Run vim in `coreboot` directory. 
Type:
`:e src/cpu/x86/16bit/reset16.inc`
Put cursor over `protected_start` and press `Ctrl-]`. If everything goes ok you 
should jump to `build/mainboard/emulation/qemu-x86/bootblock.s` line 537.  

In second article we dive into first phase of `coreboot` execution in emulated environment.