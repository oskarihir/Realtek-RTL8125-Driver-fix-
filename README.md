Realtek RTL8125 Driver Fix for Linux Kernel 6.9.3-76060903-generic
Overview

This repository provides a solution to fix the compilation issues when building the Realtek RTL8125 network driver on Linux kernel 6.9.3-76060903-generic. The primary issue occurs due to incompatible function pointer types in the driver source code, which results in build errors.
Problem Description

When attempting to build the RTL8125 driver on Linux kernel 6.9.3-76060903-generic, the following errors may occur:

bash

/home/user/r8125-9.013.02/src/r8125_n.c:7682:20: error: initialization of ‘int (*)(struct net_device *, struct ethtool_keee *)’ from incompatible pointer type ‘int (*)(struct net_device *, struct ethtool_eee *)’ [-Werror=incompatible-pointer-types]
 7682 |         .get_eee = rtl_ethtool_get_eee,
      |                    ^~~~~~~~~~~~~~~~~~~
/home/user/r8125-9.013.02/src/r8125_n.c:7682:20: note: (near initialization for ‘rtl8125_ethtool_ops.get_eee’)
/home/user/r8125-9.013.02/src/r8125_n.c:7683:20: error: initialization of ‘int (*)(struct net_device *, struct ethtool_keee *)’ from incompatible pointer type ‘int (*)(struct net_device *, struct ethtool_eee *)’ [-Werror=incompatible-pointer-types]
 7683 |         .set_eee = rtl_ethtool_set_eee,
      |                    ^~~~~~~~~~~~~~~~~~~
/home/user/r8125-9.013.02/src/r8125_n.c:7683:20: note: (near initialization for ‘rtl8125_ethtool_ops.set_eee’)

These errors arise because the function pointer types for .get_eee and .set_eee do not match the expected types in the kernel.
Solution

To resolve the issue, you need to modify the source code by casting the function pointers to the expected types. This repository contains the necessary patch and instructions to fix the issue.
Steps to Apply the Fix

    Clone the Repository:

git clone https://github.com/oskarihir/Realtek-RTL8125-Driver-fix-.git

cd rtl8125-kernel6.9.3-fix


rebuild the driver:

bash

make clean

sudo ./autorun.sh

Install and Load the Driver:

The autorun.sh script should install the driver. You can then load it using:

bash

sudo modprobe r8125

Verify the Installation:

Check if the driver is correctly installed and loaded:

bash

    lsmod | grep r8125
    ifconfig

Troubleshooting

If you encounter any issues during the build or installation process, please ensure that:

    Your system is up-to-date with all dependencies installed.
    You are using the correct kernel headers for 6.9.3-76060903-generic.

If problems persist, feel free to open an issue in this repository.
Contributing

Contributions to this repository are welcome! If you find a better solution or encounter other issues related to this driver, please open a pull request or submit an issue.
License

This project is licensed under the MIT License. See the LICENSE file for details.
