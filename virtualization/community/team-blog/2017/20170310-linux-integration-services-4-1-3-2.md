---
title:      "Linux Integration Services 4.1.3-2"
date:       2017-03-10 22:27:17
categories: uncategorized
---
Linux Integration Services has been update to version 4.1.3-2 and is available from <https://www.microsoft.com/en-us/download/details.aspx?id=51612> This is a minor update to correct the RPMs for a kernel ABI change in Red Hat Enterprise Linux, CentOS, and Oracle Linux's Red Hat Compatible Kernel version 7.3. Version 3.10.0-514.10.2.el7 of the kernel was sufficiently different for symbol conflicts to break the LIS kernel modules and create a situation where a VM would not start correctly. This version of the modules is compatible with the new kernel.
