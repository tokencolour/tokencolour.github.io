---
layout: post
title: "Community Bonding period ends"
date: 2017-05-29
---

Hey!
During the Community Bonding Period, Gedare provided detailed instructions as 
to what needs to be done during the summer.

* Begin to code as per our mentor's instructions and our proposal
* Ask them for preffered form of communication
* Introducing ourselves to the community with a mail on users mailing list
* Adding our project to [Tracking Table](https://devel.rtems.org/wiki/GSoC/2017#StudentsSummerofCodeTrackingTable)
* Adding a [Wiki](https://devel.rtems.org/wiki/GSoC/2017/RTEMSTesterImprovements) page
* Writing blog posts
* Attending all weekly meetings

We had a meeting during the Community Bonding Period and we discussed what we 
were up to and what we had done. 

I had been busy with exams and relocating since my college got over. But got a 
chance to look at the macro files and communicate with Chris and Kuan about the
format of INI files during the second half of the Community Bonding period.

We discussed why INI would be a better choice over YAML for the configuration 
files. Using PyYAML would add a dependency in the tool that would be difficult 
to handle across all hosts we support. Chris had already pushed helper code for
handling INI format. I studied it and asked the questions I had. We finalized 
on the format for ini files for different kinds of macro files.

A longer explanation for the format (and perhaps a more accurate one will be 
given in the docs), but I have a short summary here. 

The conversions are planned as below:

There are three kinds of macro files. I segregated them on the basis of 
similarity.

###### Set 1

Currently ``arm7tdmi.mc`` looks like this.

```
[global]
bsp:                 none,    none,     'arm7tdmi'

[arm7tdmi]
arm7tdmi:                 none,    none,     '%{_rtscripts}/gdb.cfg'
arm7tdmi_arch:            none,    none,     'arm'
gdb_script:          none,    none,     'arm7tdmi_gdb_script'
arm7tdmi_gdb_script:      none,    none,     '''target sim
                                           load
                                           run'''
```

We plan on changing it to INI the following way.

```
[arm7tdmi]
bsp = 'arm7tdmi'
arch = 'arm'

[gdb_script]
gdb = '%{_rtscripts}/gdb.cfg'
gdb_script ='arm7tdmi_gdb_script'
arm7tdmi_gdb_script =   'target sim
    load
    run'
```

Similarly, ``arm7tdmi-run.mc`` contains:

```
 [global]
bsp:              none,    none,     'arm7tdmi'

[arm7tdmi]
arm7tdmi:              none,    none,     '%{_rtscripts}/run.cfg'
arm7tdmi_arch:         none,    none,     'arm'
bsp_run_cmd:      none,    none,     '%{rtems_tools}/%{bsp_arch}-rtems%{rtems_version}-run'
bsp_run_opts:     none,    none,     '-a -nouartrx'
```

``arm7tdmi-run.ini`` would look like the following:

```
[arm7tdmi]
bsp = 'arm7tdmi'
arch = 'arm'

[run]
run = '%{_rtscripts}/run.cfg'
bsp_run_cmd = '%{rtems_tools}/%{bsp_arch}-rtems%{rtems_version}-run'
bsp_run_opts = '-a -nouartrx'
```

###### Set 2
If the tester should run the tests on qemu for a bsp, the given pattern can 
be followed:

```
[<bsp_name>]
bsp = '<bsp_name>'

[qemu-script]
run = '%{_rtscripts}/qemu.cfg'
arch = '<arch_name>'
opts = '%{qemu_opts_base} %{qemu_opts_no_net} -m 32M'
```

###### Set 3

We discussed specifics for configuration files which need user-specific settings.
User can also provide a "settings.ini" file in case a configuration file needs 
a path to, for instance, a first stage bootloader (fsbl) that is placed 
somewhere in the host machine. The settings.ini file can be passed with 
``rtems-test --with-settings = /path/to/settings/file`` option. The 
``<bsp_name>.ini`` can contain defaults and the settings file can contain the 
user-specified configuration.


``xilinx_zynq_zc706.ini`` should look like the follwing:

```
[xilinx_zynq_zc706]
bsp = 'xilinx_zynq_zc706'
jobs = '1'

[gdb-script]
gdb = '%{_rtscripts}/gdb.cfg'
arch = 'arm'
bsp_tty_dev =  ${settings:bsp_tty_dev}
gdb_script = 'xilinx_zynq_zc706_gdb_script'
xilinx_zynq_zc706_gdb_script = 'target remote kaka:3333
mon load_image ${settings:path_to_fsbl} 0 elf
    mon resume 0
    mon sleep 4000
    mon halt
    load
    b bsp_reset
    continue'
```

With a ``settings.ini`` which will add to the above file:

```
[settings]
bsp_tty_dev = '/dev/cu.SLAB_USBtoUART'
path_to_fsbl = '/path/to/fsbl'
```

We discussed other details too like how ConfigParser would parse the section:values.
I was asked to come up with a ReST documentation for the INI format.
Thank you Chris for patiently answering the countless stupid questions I ask.



