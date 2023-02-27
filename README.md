# Game Controllers
## Modifications
Ihis is a fork of the main WSL2 kernel that has been modified in order to use a USB gamepad controller.
I followed instructions from [this issue][xpad-issue]:
<br /> I forked the WSL2 kernel repo, cloned it to WSL2, modified a few lines of `Microsoft/config-wsl` (a Makefile) and then rebuilt the kernel
<br /> `make KCONFIG_CONFIG=Microsoft/config-wsl -j15` where the number following j is the number of threads to be used compiling the kernel
<br /> I then followed the end of [this tutorial][tutorial] to configure windows to use the new kernel for WSL2:
<br /> `cp vmlinux /mnt/c/Users/<winUserName>/`, where `winUserName` is the name of your Windows profile
<br /> `exit`
<br /> create a new file called `.wslconfig` in your user folder on windows: `C:\Users\<winUserName>\.wslconfig`
<br /> populate the file with the following:
<br /> `[wsl2]`
<br />  `kernel=C:\\Users\\<winUserName>\\vmlinux`
<br /> Then, shutdown WSL2, and hopefully next time you start it you will be running your customized kernel
<br /> In powershell `wsl --shutdown Ubuntu`
<br /> wait a few seconds, and then boot up WSL2 `ubuntu`
<br /> you can check to see what kernel you are running in bash using `uname -r` or something more extra like neofetch

## Final steps 
In order to make the gamepad be recognized, in addition to passing through the device to wsl2 using [usbipd][usbipd-instr], I also had to follow [these steps][permissions] to change the device permissions:
<br />`sudo chmod 777 /dev/input/event0`
<br />`sudo chmod 777 /dev/input/js0`

## Notes
when running `jstest-gtk` to test out the controller, I have found issues with the mappings of old XBOX controllers, such as a MadCatz. I have found the right thumbstick mapping to conflict with the right trigger, the horizontal axis of which is unresponsive. There is a program called `xboxdrv` that I am investigating and will update this README if I resolve. I don't think it's a function of WSL, as I've had this issue on normally installed Ubuntu on a different machine.

## Last notes
This particular repo's version of the WSL2 kernel may become out of date, but these instructions should still apply to future kernels. 

# WSL2-Linux-Kernel OG README
## Introduction

The [WSL2-Linux-Kernel][wsl2-kernel] repo contains the kernel source code and
configuration files for the [WSL2][about-wsl2] kernel.

## Reporting Bugs

If you discover an issue relating to WSL or the WSL2 kernel, please report it on
the [WSL GitHub project][wsl-issue]. It is not possible to report issues on the
[WSL2-Linux-Kernel][wsl2-kernel] project.

If you're able to determine that the bug is present in the upstream Linux
kernel, you may want to work directly with the upstream developers. Please note
that there are separate processes for reporting a [normal bug][normal-bug] and
a [security bug][security-bug].

## Feature Requests

Is there a missing feature that you'd like to see? Please request it on the
[WSL GitHub project][wsl-issue].

If you're able and interested in contributing kernel code for your feature
request, we encourage you to [submit the change upstream][submit-patch].

## Build Instructions

Instructions for building an x86_64 WSL2 kernel with an Ubuntu distribution are
as follows:

1. Install the build dependencies:  
   `$ sudo apt install build-essential flex bison dwarves libssl-dev libelf-dev`
2. Build the kernel using the WSL2 kernel configuration:  
   `$ make KCONFIG_CONFIG=Microsoft/config-wsl`

## Install Instructions

Please see the documentation on the [.wslconfig configuration
file][install-inst] for information on using a custom built kernel.

[usbipd-instr]: https://learn.microsoft.com/en-us/windows/wsl/connect-usb
[permissions]:  https://github.com/microsoft/WSL/issues/7747#issuecomment-1352418916
[xpad-issue]:   https://github.com/microsoft/WSL/issues/7747
[tutorial]: https://microhobby.com.br/blog/2019/09/21/compiling-your-own-linux-kernel-for-windows-wsl2
[wsl2-kernel]:  https://github.com/microsoft/WSL2-Linux-Kernel
[about-wsl2]:   https://docs.microsoft.com/en-us/windows/wsl/about#what-is-wsl-2
[wsl-issue]:    https://github.com/microsoft/WSL/issues/new/choose
[normal-bug]:   https://www.kernel.org/doc/html/latest/admin-guide/bug-hunting.html#reporting-the-bug
[security-bug]: https://www.kernel.org/doc/html/latest/admin-guide/security-bugs.html
[submit-patch]: https://www.kernel.org/doc/html/latest/process/submitting-patches.html
[install-inst]: https://docs.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig
