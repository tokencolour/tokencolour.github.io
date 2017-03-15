---
layout: post
title: "How to run applications on qemu rtems"
date: 2017-03-16
---
Hi!
My new expedition with RTEMS
My first try with running an application on rtems on target:qemu

examples from git:
git clone git://git.rtems.org/examples-v2.git examples-v2

RTEMS SOURCE BUILDER
export PATH=$HOME/development/rtems/4.12/bin:$PATH
location: /home/thd/development/rtems/src/rtems-source-builder/
execute the following command:
$ ../source-builder/sb-set-builder --log=l-i386.txt \
                --prefix=$HOME/development/rtems/4.12 4.12/rtems-i386

directory file structure
development
	rtems
		src
			rtems
			builds
			example-v2
			rtems-source-builder

now go to /development/rtems/src/rtems
execute:
./bootstrap -c && ./bootstrap -p && ./bootstrap

cd ..
then go to builds
mkdir pc386
cd pc386
execute:
../../rtems/configure --target=i386-rtems4.12 --enable-rtemsbsp=pc386 --enable-tests=samples --disable-posix --prefix=${HOME}/install_path

make
make install
cd ../../
cd example-v2
export RTEMS_MAKEFILE_PATH=/home/thd/install_path/i386-rtems4.12/pc386

export RTEMS_SHARE=/home/thd/install_path/share/rtems4.12
make

Now comes the running with QEMU part:

I first installed qemu:
sudo apt-get install qemu
sudo apt-get install qemu-system-i386

then:
I first installed qemu:
sudo apt-get install mtools

downloaded https://ftp.rtems.org/pub/rtems/people/chrisj/grub/rtems-boot.img into home/thd/floppy
i.e. an image of a floppy disk containing the grub boot loader

We run RTEMS in QEMU by first booting from a floppy disk image

dd if=/dev/zero of=hda.img count=1000
ls -las hda.img

To start the application:

sudo qemu-system-i386 -m 128 -boot a -fda ./rtems-boot.img -hda ./hda.img -hdb fat:. -append "--console=/dev/com1" -serial stdio -no-reboot -s -kernel /home/thd/development/rtems/src/examples-v2/ticker/ticker/o-optimize/ticker.exe


Tanu Hari Dixit
