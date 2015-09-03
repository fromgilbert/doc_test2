Contents

[1 Introduction 5](#_Toc417486192)

[1.1 The Yocto Project 5](#_Toc417486193)

[1.2 References 6](#_Toc417486194)

[1.3 Terminology 6](#_Toc417486195)

[2 Build an Intel® Edison Image using bitbake 7](#_Toc417486196)

[2.1 Build the Intel® Edison native SDK 8](#_Toc417486197)

[3 Build an Intel® Edison Image with make 9](#build-an-intel-edison-image-with-make)

[3.1 Build the Intel® Edison native SDK with the make command 10](#build-the-intel-edison-native-sdk-with-the-make-command)

[4 Creating Custom Intel® Edison Images 11](#_Toc417486200)

[4.1 Adding standard Yocto packages in the image 11](#_Toc417486201)

[4.2 Excluding packages from the image 11](#excluding-packages-from-the-image)

[4.3 Add third-party packages to the image 11](#_Toc417486203)

[4.4 Write a Yocto recipe from scratch 12](#_Toc417486204)

[4.5 Add a recipe for a systemd service 12](#_Toc417486205)

[5 Customizing the Linux\* Kernel 13](#_Toc417486206)

Figures

[Figure 1. Building an image 5](#_Toc417486207)

[Figure 2 Linux kernel configuration 13](#_Toc417486208)

Revision History

| Revision | Description                                                                 | Date              |
|----------|-----------------------------------------------------------------------------|-------------------|
| ww26     | Initial release.                                                            | July 7, 2014      |
| ww32     | Improved section about adding external recipes.                             | August 4, 2014    |
| ww36     | Corrected code example in chapter 4.                                        | September 5, 2014 |
| 001      | First public release.                                                       | September 9, 2014 |
| 002      | Corrected file names and file paths in section 3.3.                         | November 21, 2014 |
| 003      | Minor corrections.                                                          | December 1, 2014  |
| 004      | Minor corrections.                                                          | December 17, 2014 |
| 005      | Added chapter on using the make command to build images; minor corrections. | February 4, 2015  |
| 006      | Updated commands, based on the latest *meta-intel-edison* repository.       | May 1, 2015       |

-   

<span id="_Toc390941528" class="anchor"><span id="_Toc417486192" class="anchor"><span id="_Toc347817469" class="anchor"></span></span></span>Introduction
=========================================================================================================================================================

This document is for software and system engineers who are building and customizing images, kernels, and native SDKs for the Intel® Edison Development Platform. Precompiled versions of the BSP are available on the Intel website. Users who don’t want to modify the default images don’t need to read this document.

The offers these features:

-   Kernel image based on Linux kernel 3.10.17

-   U-boot second stage bootloader

-   Bluetooth and WiFi connectivity

-   Intel cloud connectivity middleware

-   Many base Linux packages provided by the Yocto project

    1.  <span id="_Toc390780303" class="anchor"><span id="_Toc391636884" class="anchor"><span id="_Toc417486193" class="anchor"></span></span></span>The Yocto Project
        --------------------------------------------------------------------------------------------------------------------------------------------------------------

The standard Linux\* OS shipped on the Intel® Edison platform is based on Yocto. The Yocto Project is an open source collaboration project that provides templates, tools, and methods to help you create custom Linux-based systems for embedded products.

<span id="_Toc417486207" class="anchor"></span>Figure . Building an image

| ![bsp-fig-1](https://cloud.githubusercontent.com/assets/10090748/9667039/5f8819f6-522e-11e5-9280-854c1e8ed6f0.png) |
|-------------------------|

The Intel® Edison BSP source package is the set of Yocto source files necessary to generate a Linux image ready to run on the Intel® Edison board. It contains:

-   The set of Yocto recipes describing the process for building a Linux kernel, a bootloader, and a *rootfs*, which together form the bootable images ready to flash on a device.

-   The set of Yocto recipes necessary for creating a Software Developer Kit (SDK) and a cross-compiling tool chain that developers can use to create native applications for Intel® Edison.

1.  For details on the Yocto project, consult the documentation on the Yocto website. (See section 1.2.)

    1.  <span id="_Ref394906814" class="anchor"><span id="_Toc417486194" class="anchor"></span></span>References
        --------------------------------------------------------------------------------------------------------

|           |                                                |                                                                                                                                                         |
|-----------|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Reference | Name                                           | Number/location                                                                                                                                         |
| 331188    | Intel® Edison Board Support Package User Guide | (This document)                                                                                                                                         |
| 331189    | Intel® Edison Compute Module Hardware Guide    | [http://www.intel.com/support/edison/sb/CS-035274.htm](http://www.intel.com/support/edison/sb/CS-035274.htm?wapkw=edison+compute+module+hardware+guide) |
| 331190    | Intel® Edison Breakout Board Hardware Guide    | [http://www.intel.com/support/edison/sb/CS-035252.htm](http://www.intel.com/support/edison/sb/CS-035252.htm?wapkw=edison+compute+module+hardware+guide) |
| 331191    | Intel® Edison Kit for Arduino\* Hardware Guide | [http://www.intel.com/support/edison/sb/CS-035275.htm](http://www.intel.com/support/edison/sb/CS-035275.htm?wapkw=edison+compute+module+hardware+guide) |
| 331192    | Intel® Edison Native Application Guide         | <http://www.intel.com/support/edison/sb/CS-035382.htm>                                                                                                  |
| 329686    | Intel® Galileo and Intel® Edison Release Notes | https://communities.intel.com/docs/DOC-23388                                                                                                            |
| 332032    | Intel® Edison Software Release Notes           |                                                                                                                                                         |
| \[GSG\]   | Intel® Edison Getting Started Guide            | W: https://communities.intel.com/docs/DOC-23147                                                                                                         
                                                                                                                                                                                                                       
                                                              M: <https://communities.intel.com/docs/DOC-23148>                                                                                                        
                                                                                                                                                                                                                       
                                                              L: <https://communities.intel.com/docs/DOC-23149>                                                                                                        |
| 331438    | Intel® Edison WiFi Guide                       | <http://www.intel.com/support/edison/sb/CS-035380.htm>                                                                                                  |
| 331704    | Intel® Edison Bluetooth\* Guide                | [http://www.intel.com/support/edison/sb/CS-035381.htm](http://www.intel.com/support/edison/sb/CS-035381.htm?wapkw=edison+nap)                           |
| 332434    | Intel® Edison Audio Setup Guide                |                                                                                                                                                         |
| \[YPQSG\] | Yocto Project Quick Start Guide                | [http://www.yoctoproject.org/docs/current/yocto-project-qs                                                                                              
                                                              /yocto-project-qs.html](http://www.yoctoproject.org/docs/current/yocto-project-qs/yocto-project-qs.html)                                                 |
| \[YDM\]   | Yocto Developer Manual                         | [http://www.yoctoproject.org/docs/current/dev-manual                                                                                                    
                                                              /dev-manual.html](http://www.yoctoproject.org/docs/current/dev-manual/dev-manual.html)                                                                   |
| \[YKDM\]  | Yocto Kernel Developer Manual                  | [http://www.yoctoproject.org/docs/latest/kernel-dev                                                                                                     
                                                              /kernel-dev.html](http://www.yoctoproject.org/docs/latest/kernel-dev/kernel-dev.html)                                                                    |

<span id="_Toc417486195" class="anchor"><span id="_Toc389741257" class="anchor"><span id="_Toc390780302" class="anchor"><span id="_Toc391636883" class="anchor"><span id="_Toc112736949" class="anchor"><span id="_Toc125788474" class="anchor"></span></span></span></span></span></span>Terminology
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

| Term | Definition             |
|------|------------------------|
| SSH  | Secure shell           |
| FTP  | File Transfer Protocol |
| GDB  | GNU debugger           |

-   

<span id="_Toc390780305" class="anchor"><span id="_Toc391636886" class="anchor"><span id="_Toc392499322" class="anchor"><span id="_Toc417486196" class="anchor"></span></span></span></span>Build an Intel® Edison Image using *bitbake*
========================================================================================================================================================================================================================================

Building a standard Intel® Edison image requires downloading and installing several prerequisite packages. These instructions are valid for a recent Ubuntu Linux\* distribution and should be valid for other distributions with minor changes.

1.  Make sure your working directory is not part of an encrypted file system, such as **eCryptFS**. Because encrypted file systems restrict file length, the build will fail.

To build a standard Intel® Edison image, do the following:

1.  Install the prerequisite packages with the following command:

sudo apt-get install build-essential git diffstat gawk chrpath texinfo libtool gcc-multilib

1.  Download the BSP source package *edison-src.tgz* from the [Intel® Edison Software Downloads ](https://software.intel.com/iot/hardware/edison/downloads)page. The package includes the full Yocto environment, and Intel® Edison-specific Yocto recipes to build the image (including the Linux kernel), a bootloader, and all necessary packages. Download the BSP source package to your working directory and decompress it.

tar xvf edison-src.tgz

cd edison-src/

1.  Optionally, you can move your download and build cache (also called *sstate*) directories from the default location under the build directory, using the *--dl\_dir* and *--sstate\_dir* options. Doing this will make it easier to share this data between build environments, and allow much faster build and download time when rebuilding the full image, even after a full manual cleanup (by means of deleting everything under your build directory). To create the *sstate* directory, use the *mkdir* command:

mkdir bitbake\_download\_dir

mkdir bitbake\_sstate\_dir

1.  Configure the shell environment with the *source* command below. After the command executes, the directory changes to the *edison-src/build* folder.

source poky/oe-init-build-env

1.  Now you are ready to build the full Intel® Edison image with the *bitbake* command:

bitbake edison-image

Building all the packages from scratch can take up to 5 or 6 hours, depending on your host. After the first build (provided you have not done any major cleanups), you can expect much faster rebuilds, depending on your host and the amount of changes. When the bitbake process completes, images to flash are created in the *edison-src/build/tmp/deploy/images/edison* directory. To simplify the flash procedure, run the script below to copy the necessary files to the *build/toFlash* directory.

../meta-intel-edison/utils/flash/postBuild.sh

The images are ready to flash on the Intel® Edison Development Board. Refer to the \[GSG\] for details on the flashing procedure.

<span id="_Toc390780306" class="anchor"><span id="_Toc391636887" class="anchor"><span id="_Toc417486197" class="anchor"></span></span></span>Build the Intel® Edison native SDK
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

To cross-compile native applications for your image, you must generate an SDK containing a cross-compiler toolchain and sysroot. You can generate a full SDK for the Intel® Edison Development Board with the following command:

bitbake edison-image -c populate\_sdk

This *bitbake* command creates the SDK installer script:

ls tmp/deploy/sdk

poky-edison-eglibc-x86\_64-edison-image-core2-32-toolchain-1.6.1.sh

-   

Build an Intel® Edison Image with *make*
========================================

The Intel® Edison source also supports Makefile-based builds, which allows you to build a standard Intel® Edison image with the *make* command. To build a standard image with the *make* command, do the following:

1.  Install the prerequisite packages (if you have not already done so) with the following command:

sudo apt-get install build-essential git diffstat gawk chrpath texinfo libtool gcc-multilib

1.  Download the BSP source package *edison-src.tgz* from the [Intel® Edison Software Downloads ](https://software.intel.com/iot/hardware/edison/downloads)page. The package includes the full Yocto environment, and Intel® Edison-specific Yocto recipes to build the image (including the Linux kernel), a bootloader, and all necessary packages. Download the BSP source package to your working directory and decompress it.

tar xvf edison-src.tgz

cd edison-src/

1.  To configure the Yocto build environment, enter the following command:

make setup

This command automatically creates *download* and *sstate* directories in the *bbcache* directory. You can override the value for the download cache directory or specify the number of threads to use for compilation (to speed up build time) by editing the Yocto configuration file *local.conf.* This file is in *&lt;edison-src&gt;/out/linux64/build/conf/local.conf*.

1.  When the setup is ready, you can build the full Intel® Edison image with the *make* command:

make image

You will find the images in the *&lt;edison-src&gt;/out/current/build/toFlash* directory.

1.  Flash the images:

make flash

To get help on available commands, enter the following:

make help

Build the Intel® Edison native SDK with the make command
--------------------------------------------------------

To cross-compile native applications for your image, you must generate an SDK containing a cross-compiler toolchain and sysroot. You can generate a full SDK for the Intel® Edison Development Board with the following command:

make sdk

This *make* command creates the SDK installer script in *&lt;edison-src&gt;/out/current/build/tmp/deploy/sdk*:

ls out/current/build/tmp/deploy/sdk/

poky-edison-eglibc-x86\_64-edison-image-core2-32-toolchain-1.6.1.sh

Execute the cross-compiler script to install the toolchain.

You can still use the *bitbake* command if you used *make* for basic setup as described above. In order to use the *bitbake,* go to the *&lt;edison-src&gt;/out/current/* directory and configure the shell environment with the *source* command, as shown below.

$ cd out/current

$ source poky/oe-init-build-env

After the command executes, the directory changes to the *edison-src/out/linux64/build* folder.

-   

<span id="_Toc390780307" class="anchor"><span id="_Toc391636888" class="anchor"><span id="_Toc417486200" class="anchor"></span></span></span>Creating Custom Intel® Edison Images
=================================================================================================================================================================================

This section explains how to customize standard Linux\* images for the Intel® Edison platform.

<span id="_Ref394383224" class="anchor"><span id="_Toc417486201" class="anchor"></span></span>Adding standard Yocto packages in the image
-----------------------------------------------------------------------------------------------------------------------------------------

Yocto comes with a large set of recipes allowing you to simply add packages to our image. The available packages are on [http://packages.yoctoproject.org](http://packages.yoctoproject.org/). In order to add a package to our image, you simply need to add it to the *IMAGE\_INSTALL* variable. For example, if you want to add the lib PNG to the image, add the following line to the *edison-src /meta-intel-edison/meta-intel-edison-distro/recipes-core/images/edison-image.bb* file:

IMAGE\_INSTALL += “libpng”

Rebuild the image to have *libpng* included in it.

1.  If you need to add patches to existing upstream sources, consult the Yocto documentation \[YDM\].

    1.  Excluding packages from the image
        ---------------------------------

To exclude unnecessary packages from the image, remove the matching entry from the *IMAGE\_INSTALL* variable (see section 4.1) or add the package name to the *PACKAGE\_EXCLUDE* variable in the *build/conf/local.conf* file.

PACKAGE\_EXCLUDE = "package1 package2"

<span id="_Toc390780308" class="anchor"><span id="_Toc391636889" class="anchor"><span id="_Toc417486203" class="anchor"></span></span></span>Add third-party packages to the image
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

If Yocto does not provide a package you need, chances are good that someone else has created a Yocto recipe for it. In this section, we will add a set of Yocto recipes (from *meta-openembedded,* a third-party Yocto layer) to the Intel® Edison source. The recipes contained in this layer allow you to add many packages in a custom Intel® Edison image. Download the *meta-openembedded* layer at: <https://github.com/openembedded/meta-openembedded>. As an example, the *opencv* library will be added to the image. The example assumes a standard image has been created by running the *setup.sh* script and *bitbake edison-image* as described in the previous sections.

1.  Get the OpenEmbedded Yocto layer collection from GitHub. We use the “daisy” branch matching the version of Yocto that is used by the Intel® Edison software.

cd edison-src/meta-intel-edison

git clone https://github.com/openembedded/meta-openembedded.git

cd meta-openembedded

git checkout daisy

1.  Tell bitbake to look for recipes contained in the new meta-openembedded layer. Edit the *edison-src /build/conf/bblayers.conf* file and append the path to the new layer into the BBLAYERS variable:

BBLAYERS ?= " \\

\[..\]

Full/path/to/edison-src/meta-intel-edison/meta-openembedded/
 meta-openembedded \\ "

1.  You now can add any recipe provided by the new *meta-oe* layer to your image. As in section 4.1, to add *opencv* to the image, add it to the *IMAGE\_INSTALL* variable. You can do this in the *edison-src /meta-intel-edison/meta-intel-edison-distro/recipes-core/images/edison-image.bb* file, for example. In the particular case of *opencv*, to avoid bringing too many dependencies, you should also redefine a specific variable so that the library is built without *gtk* support:

IMAGE\_INSTALL += “opencv”

PACKAGECONFIG\_pn-opencv="eigen jpeg libav png tiff v4l”

1.  Save the file and rebuild the image as follows:

<span id="_Toc390780309" class="anchor"><span id="_Toc391636890" class="anchor"></span></span>cd edison-src

source poky/oe-init-build-env

bitbake edison-image

<span id="_Toc390780311" class="anchor"><span id="_Toc391636892" class="anchor"><span id="_Toc417486204" class="anchor"></span></span></span>Write a Yocto recipe from scratch
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

It is also possible to create your own Yocto recipes from scratch and add them to the image. This section describes the required steps to add a *hello\_world* C program to our image. The GNU *hello\_world* is a real project that you can download from <http://ftp.gnu.org/gnu/hello/hello-2.7.tar.gz>.

1.  The first step is to tell *bitbake* where to download the code, and how to build the package. This is done by adding a new recipe (.bb) file in the right directory. To do this, create the recipe file *hello\_2.7.bb* in the *edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/hello* directory, with the following content:

DESCRIPTION = “GNU Helloworld application”

LICENSE = "GPLv3+"

LIC\_FILES\_CHKSUM =“file://COPYING;md5=d32239bcb673463ab874e80d47fae504”

SRC\_URI = “${GNU\_MIRROR}/hello/hello-${PV}.tar.gz”

SRC\_URI\[md5sum\] = “fc01b05c7f943d3c42124942a2a9bb3a”

inherit autotools gettext

1.  As the *hello\_world* project makes use of the autotools, it is enough to inherit the autotool Yocto class to tell bitbake how to configure and build the project. Refer to the \[YDM\] for details on the *.bb* syntax.

<!-- -->

1.  The *hello world* recipe is ready, but you still need to add it to your image. To do so, add the following line to the *edison-src /meta-intel-edison/meta-intel-edison-distro/recipes-core/images/edison-image.bb* file:

IMAGE\_INSTALL += “hello”

1.  Then rebuild the image:

bitbake edison-image

<span id="_Toc390780312" class="anchor"><span id="_Toc391636893" class="anchor"><span id="_Toc417486205" class="anchor"></span></span></span>Add a recipe for a systemd service
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Developers may choose to add their own application as a service to Intel® Edison. On the Intel® Edison platform, services are special applications that run in the background. They are managed by *systemd*, a system and service manager for Linux\*.

A *systemd* service is described by a .service file that needs to be deployed on the Intel® Edison board usually in */lib/systemd/system*. This service file contains information on how and when to start the service, which are its dependencies, etc.

1.  Refer to *systemd* documentation [http://www.freedesktop.org/wiki/Software/systemd](http://www.freedesktop.org/wiki/Software/systemd/) for an overview of the base *systemd* concepts, and a description of the associated tools.

The Intel® Edison BSP source includes a sample recipe for creating a *systemd* service application using Yocto. The sample is in the *edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/watchdog-sample* folder.

A system service is described by a *.service* file. Refer to the sample file *watchdog-sample.service* at: <http://www.freedesktop.org/software/systemd/man/systemd.service.html>.

To deploy the service from a Yocto recipe, you need to inherit the Yocto systemd class. Refer to [http://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html\#ref-classes-systemd](http://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html%23ref-classes-systemd%20).

-   

<span id="_Toc390780313" class="anchor"><span id="_Toc391636894" class="anchor"><span id="_Toc417486206" class="anchor"></span></span></span>Customizing the Linux\* Kernel
===========================================================================================================================================================================

This chapter contains a brief overview of making kernel modifications. Customizing the kernel is important on embedded systems for making new devices and sensors. For more detailed information, see the \[YKDM\] at: <http://www.yoctoproject.org/docs/latest/kernel-dev/kernel-dev.html>. Check it out for additional ways of configuring the kernel, for example through using more compact and modular configuration fragments. The approach described here is good for ad-hoc modifications while config fragments are shorter than full kernel configuration, and it allows you to create and distribute your own Yocto recipes for modifying specific kernel features.

The base kernel config file is delivered with *edison-src.tar.gz* and is located in the *edison-src/meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/files/defconfig* file.

The *menuconfig* tool provides an easy interactive method with which to define kernel configurations. For general information on *menuconfig*, see <http://en.wikipedia.org/wiki/Menuconfig>. The following command opens the *menuconfig* terminal for configurations:

bitbake virtual/kernel -c menuconfig

<span id="_Toc417486208" class="anchor"></span>Figure 2 Linux kernel configuration

| ![bsp-fig-2](https://cloud.githubusercontent.com/assets/10090748/9667052/705b3b1e-522e-11e5-9d01-ca1b8c0db84c.png) |
|-------------------------|

When the configuration is completed, replace *defconfig* with *.config*, then rename it back to *defconfig*. We also suggest taking a backup of the *defconfig* file. Force bitbake to copy the modified *defconfig* file to the actual build directory. Then the new image with modified kernel is ready to build.

cp &lt;path\_to\_edison-src&gt;/build/tmp/work/edison-poky-linux/linux- yocto/3.10.17+gitAUTOINC+6ad20f049a\_c03195ed6e-r0/linux-edison-standard-build/**.config** &lt;path\_to\_edison-src&gt;/meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/files/**defconfig**

We can also change the Intel® Edison kernel configuration (i386\_edison\_defconfig) file and overwrite with customized kernel configuration by doing following:

<span id="_Toc390780314" class="anchor"><span id="_Toc391636895" class="anchor"></span></span>cp &lt;path\_to\_edison-src&gt;/build/tmp/work/edison-poky-linux/linux- yocto/3.10.17+gitAUTOINC+6ad20f049a\_c03195ed6e-r0/linux-edison-standard-build/**.config** &lt;path\_to\_edison-src&gt;/build/tmp/work/edison-poky-linux/linux-yocto/3.10.17+gitAUTOINC+6ad20f049a\_c03195ed6e-r0/linux/arch/x86/configs/**i386\_edison\_defconfig**

bitbake virtual/kernel –c configure –f –v

bitbake edison-image

-
