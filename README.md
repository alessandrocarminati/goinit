# goinit
## Introduction
`goinit` is a task-specific Linux init implementation aimed at mimicking the unikernel strategy within a traditional Linux kernel environment. 
It is designed to execute "flashing" actions, such as fetching and writing the root filesystem, boot items like device tree, U-Boot environment variables, or kernel image. 
The project's goal is to simplify and standardize these tasks within a Linux environment.
`goinit` is integrated into the Linux kernel and replaces the typical initramfs. 
It can be embedded into the kernel image, allowing U-Boot to fetch and execute the kernel. 
U-Boot controls `goinit` using kernel arguments in the format `pr.<command>=value`.

## Build
To build the `goinit` project, follow these steps:

Download the Repository: Clone the `goinit` repository from the source repository:

```
git clone https://github.com/alessandrocarminati/goinit
```
Replace `https://github.com/alessandrocarminati/goinit` with the URL of the repository.

Build the Project: Navigate to the root directory of the cloned repository and run the following command to build the project:

```
make
```

This command compiles the source code and generates the necessary binaries.
Retrieve the Root Filesystem `rootfs.cpio`: After the build process completes, you can find the root filesystem `rootfs.cpio` in the `bin` directory of the project.
The `rootfs.cpio` file contains the root filesystem required for `goinit` operation.

With these steps completed, you have successfully built the `goinit` project and obtained the root filesystem required for further usage.

## Kernel Integration
For `goinit` to be functional, it must be integrated with a working kernel for the board. 
The Linux kernel provides a configuration option that facilitates this integration. 
Ensure that your kernel configuration includes the following items:

```
CONFIG_INITRAMFS_SOURCE="<path to the cpio>"
CONFIG_INITRAMFS_ROOT_UID=0
CONFIG_INITRAMFS_ROOT_GID=0
```
* CONFIG_INITRAMFS_SOURCE: This option specifies the path to the root filesystem (rootfs.cpio) generated by "goinit" during the build process.
* CONFIG_INITRAMFS_ROOT_UID: Sets the UID (user identifier) of the root user in the initramfs to 0 (root).
* CONFIG_INITRAMFS_ROOT_GID: Sets the GID (group identifier) of the root user in the initramfs to 0 (root).

By configuring the kernel with these options, the resulting kernel image will include `goinit` embedded within it. 
This kernel image will be recognized as a "U-Boot executable," allowing U-Boot to fetch and execute it accordingly.
Ensure that you build the kernel with these configurations to produce a kernel image that incorporates "goinit" seamlessly.

## Usage
Supported Kernel Arguments
* `pr.ifname`: Specifies the interface to bring up (supports DHCP only).
* `pr.syslogIP`: Specifies the syslog server to send message logs.
* `pr.action`: Specifies the action to execute (e.g., flashRootfs, writeKernel, writeDTB, writeUbootEnv, Reboot, etc.).
* `pr.actionArgx`: Represents arguments passed to the action specified by pr.action.
* `pr.debuglevel`: Adjusts the debug level of goinit.
* `pr.reboot`: Instructs goinit to reboot the system after completing its tasks.
[Insert additional usage instructions or examples here]

`goinit` is developed as part of the automation process for the provisioner tool, serving as the provisioner agent on the board.

This documentation provides an overview of the "goinit" project, including its purpose, build instructions, integration with the Linux kernel, and usage details.