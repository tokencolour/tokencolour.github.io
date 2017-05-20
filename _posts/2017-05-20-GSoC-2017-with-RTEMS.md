---
layout: post
title: "GSoC 2017 with RTEMS"
date: 2017-05-20
---

![GSoC Logo](index.png)

I have been selected as a participant for [Google Summer of Code, 2017](https://summerofcode.withgoogle.com/) under [RTEMS project](https://www.rtems.org/). My proposal for the project can be found here. My project involves bringing about enhancements in [rtems-tester](https://ftp.rtems.org/pub/rtems/people/chrisj/rtems-tester/rtems-tester.html). 

RTEMS​ is an embedded operating system and is cross-compiled for a number of target
boards. Testing RTEMS requires that the cross-compiled test executable is transferred to the
target hardware, executed and the output returned to the host where it is analysed to
determine the test result. The RTEMS Tester​ provides a framework to do this. The goal of
this project is to improve the current facilities of rtems-tester.

RTEMS-Tester is a test framework which can execute tests not only those related to RTEMS
but also for any suitable application. The RTEMS testing tool resides in the RTEMS Tools
package and this project aims at enhancing and improving some of the existing functionality
and to complete needed parts. The following are the improvements suggested and why I
think they will be beneficial:
1. As of now, the configuration file format is a simple text file (with .mc extension) that
serves to provide all the needed macros. This is really cryptic and needs to be
converted to a suitable configuration format. I believe if I could add support for
configuration files in YAML format, which will make it more readable, it would come
really handy for developers to understand and tune them to their needs.
2. Also, sometimes a BSP configuration needs some user-specific data to function as
expected. For example, look at this​​ macro file. Here, the path to the first stage
bootloader (fsbl) has to be provided that is placed somewhere in the host machine.
For this there should be proper control provided to the user as opposed to the hard
coded configuration that is presently the case. This will help in the tunability of the
configuration files.
3. Currently, the tests that are expected to fail, the results of which are indeterminate
and those which timeout because they expect input from user, all are counted as
failed (or timeouts in the last case). This masks the actual reasons why a certain test
failed (or timed out) and eventuate into test statistics that can be easily
misunderstood. Therefore, one objective of the project is to add support for test
states “expected-fail”, “indeterminate” and “user-input”, so that they can be tracked
separately.
4. Furthermore, regression analysis is another objective of the project. Adding YAML
files for each bsp that contain an expected statistic for the tests, shall help the
cause. Hence, this statistic can be compared with the current number of failed tests
and help determine if the BSP has regressed against a change in rtems (or the test
results in RTEMS need updating.)
5. The present console support in the back end of rtems-tester is given by termios​.
This python library does not work on Windows​ . Hence, my goal is to integrate
PySerial​​ so that the benefits of it being a cross-platform tool help support
consoles on both Windows and Unix.
6. Ser2net​​ is used for providing remote access to a unit’s server ports over telnet. It
is proposed to add support for a telnet console in rtems-tester so that it becomes
convenient to telnet into serially connected devices and test applications on them,
remotely.
7. I’ll add simulator recipes for RTEMS to work with gem5​ for sparc64/usiii and
arm/realview_pbx_a9_qemu BSPs.
8. I also plan to export the results of rtems-tester to xml format so that it can be
plotted easily and used for other analysis as developers and users deem fit.

