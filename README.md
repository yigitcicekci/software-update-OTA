# SOFTWARE UPDATE AND IoT DEPLOYMENT SOLUTIONS RESEARCH

## OSTree

### What is the OSTree?
This project is now known as “libostree”, though it is still appropriate to use the previous name: “OSTree” (or “ostree”). The focus is on projects which use libostree’s shared library, rather than users directly invoking the command line tools (except for build systems). However, in most of the rest of the documentation, we will use the term “OSTree”, since it’s slightly shorter, and changing all documentation at once is impractical. We expect to transition to the new name over time.

As implied above, libostree is both a shared library and suite of command line tools that combines a “git-like” model for committing and downloading bootable filesystem trees, along with a layer for deploying them and managing the bootloader configuration.

The core OSTree model is like git in that it checksums individual files and has a content-addressed-object store. It’s unlike git in that it “checks out” the files via hardlinks, and they thus need to be immutable to prevent corruption. Therefore, another way to think of OSTree is that it’s just a more polished version of Linux VServer hardlinks.

###  How to use OSTree in IoT Deployment?

OSTree can be used in IoT deployment to provide atomic updates, rollback, and parallel installing of multiple bootable roots for Linux-based operating systems. OSTree can replicate content incrementally over HTTP via GPG signatures and “pinned TLS” support1.

Some examples of IoT projects that use OSTree are:

- Fedora IoT, which uses rpm-ostree to manage the host system and Flatpak to manage applications. Fedora IoT also provides a tool called otto that helps ship ostree commits inside Docker/OCI containers and run a webserver to serve the commits.
- Intel IoT Reference OS Kit (refkit), which has support for updating devices running one of the refkit profile images using an HTTP/HTTPS server and OSTree.

### Benefits For OSTree
- Atomic updates: OSTree can create deployments and atomically insert them into the boot partition, ensuring that the system is always in a consistent and working state. It also allows for systemwide rollback by selecting old deployments during startup.
- Efficiency: OSTree uses a content-addressed object store with branches to store file system trees. This means that only the changes between different versions are stored and transferred, saving disk space and bandwidth. OSTree also supports deduplication of files across different branches and deployments.
- Security: OSTree ensures the integrity of the files and deployments by using checksums and GPG signatures. It also makes the files immutable by mounting the filesystem as read-only, preventing unauthorized or accidental modifications.
- Flexibility: OSTree allows for parallel installing of multiple bootable roots, which can be useful for testing or experimenting with different versions or configurations. It also provides a mechanism to allow filesystem trees to add configuration files to /etc while also allowing system administrators to edit those files in a persistent manner.

## Aktualizr

### What is the Aktualizr?
C++ implementation of Uptane OTA update client.

The client is intended to be installed on devices that wish to receive OTA updates from an Uptane-compatible OTA server such as HERE OTA Connect. It is most commonly built by using the meta-updater layer in a Yocto environment. You can use aktualizr as a stand-alone system tool or you can integrate libaktualizr into a larger project.

The client is responsible for:

- Communicating with the OTA server

- Authenticating using locally available device and user credentials

- Reporting current software and hardware configuration to the server

- Checking for any available updates for the device

- Downloading any available updates

- Installing the updates on the system, or notifying other services of the availability of the downloaded file

- Receiving or generating installation reports (success or failure) for attempts to install received software

- Submitting installation reports to the server

The client maintains the integrity and confidentiality of the OTA update in transit, communicating with the server over a TLS link. The client can run either as a system service, periodically checking for updates, or can by triggered by other system interactions (for example on user request, or on receipt of a wake-up message from the OTA server).

### How to use Aktualizr in IoT Deployment?
Aktualizr can be used in IoT deployment to provide secure and reliable updates for Linux-based devices. It can communicate with the OTA server, authenticate using locally available device and user credentials, report current software and hardware configuration, check for any available updates, download and install the updates, and submit installation reports to the server.

To use aktualizr in IoT deployment, you will need to:

- Build aktualizr from source or use a pre-built package for your platform.
- Configure aktualizr with the appropriate settings for your device and server.
- Provision your device with the necessary credentials and metadata.
- Run aktualizr as a system service or trigger it manually.

### Benefits of Aktualizr
- Security: Aktualizr implements the Uptane security framework, which provides strong protection against various attacks on the update process. It can authenticate, encrypt, verify, and sign the updates using locally available device and user credentials.
- Reliability: Aktualizr can report, check, download, and install updates, and submit installation reports to the server. It can handle errors, failures, and conflicts gracefully and transparently. It can also resume interrupted downloads and apply partial updates.
- Flexibility: Aktualizr can be configured with various settings for the device and server using .toml configuration files. It can also be integrated with different tools and services, such as Docker, OSTree, QtOTA, TorizonCore, and HERE OTA Connect.
- Extensibility: Aktualizr is based on libaktualizr, a C++ library that provides the core functionality for OTA Connect. Libaktualizr can be used to create custom applications or wrappers that suit different needs and preferences.

## Aktualizr-lite

### What is the Aktualizr-lite?
Aktualizr-lite is a C++ implementation of TUF OTA update client based on aktualizr. It is targeting users who wish to have the security aspects of TUF but do not want the complexity of Uptane.

There are two modes aktualizr-lite supports:

- Anonymous mode: This mode allows devices to download updates from a public stream without registering or paying for an OTA service. This is useful for testing or evaluation purposes.

- Daemon mode: This mode allows aktualizr-lite to run as a daemon periodically checking for new updates to apply. It also supports tags, reboot-command, and update-lockfile features that can be configured by the user.

### How to use Aktualizr-lite in IoT deployment?
Aktualizr-lite can be used in IoT deployment to provide secure and reliable updates for Linux-based devices. It can communicate with the OTA server, authenticate using locally available device and user credentials, report current software and hardware configuration, check for any available updates, download and install the updates, and submit installation reports to the server.

To use aktualizr-lite in IoT deployment, you will need to:

- Build aktualizr-lite from source or use a pre-built package for your platform.
- Configure aktualizr-lite with the appropriate settings for your device and server.
- Provision your device with the necessary credentials and metadata.
- Run aktualizr-lite as a system service or trigger it manually.
- You can also use aktualizr-lite in anonymous mode or daemon mode, depending on your needs and preferences.

### Benefits of Aktualizr-lite
- Simplicity: Aktualizr-lite is targeting users who wish to have the security aspects of TUF but do not want the complexity of Uptane. It does not require device registration or management, and it does not use the Director and Director daemon components from OTA Connect.
- Flexibility: Aktualizr-lite supports two modes: anonymous mode and daemon mode. Anonymous mode allows devices to download updates from a public stream without registering or paying for an OTA service. Daemon mode allows aktualizr-lite to run as a daemon periodically checking for new updates to apply. It also supports tags, reboot-command, and update-lockfile features that can be configured by the user.
- Extensibility: Aktualizr-lite is based on libaktualizr, a C++ library that provides the core functionality for OTA Connect. Libaktualizr can be used to create custom applications or wrappers that suit different needs and preferences1 . Aktualizr-lite also integrates with Foundries.io Factory, a platform that enables secure software development and deployment for IoT devices.


## QtOTA
### What is the QtOTA?
The Qt OTA offering provides tools that assist in enabling OTA update functionality in an embedded linux images build with meta-boot2qt. Generating new updates for OTA enabled devices is completely automated, given an ordinary linux sysroot as an input.

 This includes OTA updates for linux kernel, system libraries, user space applications, translation fixes, anything that is part of the sysroot. The offering includes Qt/C++ and QML APIs to make integration with your Qt-based application a breeze.

 
### How to use QtOTA in IoT Deployment?
To use QtOTA in IoT deployment, you will need to:

- Install QtOTA on your host machine and your target device.
- Configure QtOTA with the appropriate settings for your device and server.
- Create an OSTree repository and upload your Qt application to it.
- Create an OTA update campaign using Aktualizr or OTA Connect.
- Monitor the update progress and status using Aktualizr or OTA Connect.

### Benefits of QtOTA
- Compatibility: QtOTA is based on OSTree and Aktualizr, which are widely used and supported systems for versioning and updating Linux-based operating systems. It also integrates with Qt Creator IDE and Qt Device Creation, which are popular tools for developing and deploying Qt applications.
- Security: QtOTA implements the Uptane security framework, which provides strong protection against various attacks on the update process. It can authenticate, encrypt, verify, and sign the updates using locally available device and user credentials.
- Reliability: QtOTA can create deployments and atomically insert them into the boot partition, ensuring that the system is always in a consistent and working state. It also allows for systemwide rollback by selecting old deployments during startup. It can handle errors, failures, and conflicts gracefully and transparently.
- Flexibility: QtOTA can be configured with various settings for the device and server using .toml configuration files. It can also be customized with different scripts and hooks to suit different needs and preferences.

## Torizon
### What is the Torizon?
Torizon is a Linux-based software platform that simplifies the development and operation of embedded IoT devices. It allows you to configure the system for your use case quickly and easily, so you can focus on application development instead of Linux builds.

Torizon is based on TorizonCore, a ready-to-use industrial Linux distribution that works out of the box on Toradex System on Modules (SoMs). TorizonCore supports software containerization using Docker, which makes integrating third-party software easy and provides an additional layer of robustness.

Torizon also offers various features and tools to make developers more productive and help them create easy-to-maintain products, such as:

- Visual Studio Code Integration: The simplest way to develop, deploy and debug your application using Qt, C/C++, Python or .NET Core.
- Remote Updates: A free, seamlessly integrated end-to-end, over-the-air update solution built into Torizon2.
- Device Monitoring: A tool to analyze performance, health and potential issues on your devices.
- TorizonCore Builder: A tool to easily customize Torizon with device configuration, preinstalled containers, settings, and more.
- Software Ecosystem: A large ecosystem of partners and third-party software integrations for Torizon, such as Qt, Codesys, Crank and Uno.

### How to use Torizon in IoT Deployment?
To use Torizon in IoT deployment, you will need to:

- Install TorizonCore on your Toradex System on Module (SoM) using the Toradex Easy Installer.
- Configure TorizonCore with the appropriate settings for your device and server using TorizonCore Builder or Visual Studio Code.
- Develop your application using Qt, C/C++, Python or .NET Core and package it as a Docker container.
- Deploy your application and system updates to your device using Remote Updates or Offline Updates.
- Monitor your device performance, health and potential issues using Device Monitoring.

You can also use Torizon in IoT deployment with various third-party software and services, such as:

- AWS IoT Greengrass: A service that extends AWS to edge devices so they can act locally on the data they generate, while still using the cloud for management, analytics, and durable storage.
- Azure IoT Edge: A service that delivers cloud intelligence locally by deploying and running artificial intelligence (AI), Azure services, and custom logic directly on cross-platform IoT devices.
- Codesys Edge Gateway: A software that enables the communication between Codesys controllers and Codesys Automation Server, a cloud-based platform that provides comprehensive functionality for industrial automation.

### Benefits of Torizon
- Productivity: Torizon makes developers more productive and helps them create easy-to-maintain products. It allows you to configure the system for your use case quickly and easily, so you can focus on application development instead of Linux builds. It also integrates with popular tools like Visual Studio Code, Qt, Codesys, Crank and Uno.
- Security: Torizon is designed for the most demanding applications. It implements the Uptane security framework, which provides strong protection against various attacks on the update process. It also supports software containerization using Docker, which adds an additional layer of robustness.
- Reliability: Torizon is based on TorizonCore, a ready-to-use industrial Linux distribution that works out of the box on Toradex System on Modules (SoMs). TorizonCore supports atomic updates, rollback, and parallel installing of multiple bootable roots using OSTree and Aktualizr.
- Flexibility: Torizon offers various features and tools to make developers more productive and help them create easy-to-maintain products, such as Remote Updates, Device Monitoring, TorizonCore Builder, and Software Ecosystem. Torizon also supports a large ecosystem of partners and third-party software integrations.

## FullMetalUpdate
### What is the FullMetalUpdate?
FullMetalUpdate is an open-source OTA (Over The Air) update solution for Linux-based IoT devices. It is based on OSTree and Docker, and supports delta updates, cloud agnosticism, and unbrickable devices.

FullMetalUpdate consists of a Python client application that runs on the device and communicates with a server that hosts the OSTree repository and the Docker images. The server can be either a custom one or a third-party service such as Hawkbit.

### How to use FullMetalUpdate in IoT Deployment?
To use FullMetalUpdate in IoT deployment, you will need to:

- Install FullMetalUpdate on your device using the meta-layer for Yocto Project or the pre-built images.
- Configure FullMetalUpdate with the appropriate settings for your device and server using the configuration file or environment variables.
- Create an OSTree repository and upload your Linux distribution and Docker images to it using the scripts provided by FullMetalUpdate.
- Create an OTA update campaign using Hawkbit or your own server and deploy it to your device.
- Monitor your device status and logs using Hawkbit or your own server.

### Benefits of FullMetalUpdate
- Security: FullMetalUpdate is based on OSTree and Docker, which are widely used and supported systems for versioning and updating Linux-based operating systems. It can authenticate, encrypt, verify, and sign the updates using locally available device and user credentials.
- Reliability: FullMetalUpdate can create deployments and atomically insert them into the boot partition, ensuring that the system is always in a consistent and working state. It also allows for systemwide rollback by selecting old deployments during startup. It can handle errors, failures, and conflicts gracefully and transparently.
- Efficiency: FullMetalUpdate supports delta updates, which means that only the changes between different versions are stored and transferred, saving disk space and bandwidth. It also supports software containerization using Docker, which makes integrating third-party software easy and provides an additional layer of robustness.
- Flexibility: FullMetalUpdate is cloud-agnostic, which means that it can work with any cloud service or server that hosts the OSTree repository and the Docker images. It also supports deep learning and edge AI inference models, which can be updated easily using FullMetalUpdate.

## Rpm-ostree
### What is the rpm-ostree?
rpm-ostree is a hybrid image/package system that combines libostree and RPM. It allows you to manage your operating system as a whole, while also providing the flexibility to install individual packages on top of it.

rpm-ostree works by creating an immutable base image from a set of RPM packages, and applying it atomically to the root filesystem using libostree. You can then layer additional RPM packages on top of the base image, or override existing packages with newer versions. You can also roll back to previous states of the system if something goes wrong.

rpm-ostree is used by various Linux operating systems and tools, such as Fedora Silverblue, Fedora IoT, Fedora CoreOS, and Red Hat Enterprise Linux Atomic Host.

### How to use rpm-ostree in IoT Deployment?
rpm-ostree is a tool that allows you to manage your Linux-based IoT devices using a hybrid image/package system. It uses libostree to atomically replicate a base OS image from a server, and then lets you layer additional RPM packages on top of it. You can also roll back to previous states of the system if something goes wrong.

To use rpm-ostree in IoT deployment, you will need to:

- Install rpm-ostree on your device using the appropriate distribution, such as Fedora IoT, Fedora Silverblue, Fedora CoreOS, or Red Hat Enterprise Linux Atomic Host.
- Configure rpm-ostree with the appropriate settings for your device and server using the configuration file or command-line options.
- Create an OSTree repository and upload your base OS image and RPM packages to it using the commands provided by rpm-ostree.
- Deploy your base OS image and layered packages to your device using the rpm-ostree upgrade command.
- Monitor your device status and logs using the rpm-ostree status command or other tools.

### Benefits of Rpm-ostree
- Atomic updates: rpm-ostree uses OSTree to provide atomic, read-only file system images that are designed to be deployed and updated as a single unit. This means that updates to the operating system are applied as a whole, rather than package by package, reducing the likelihood of conflicts and ensuring a consistent system state.
- Security: rpm-ostree ensures the integrity of the files and deployments by using checksums and GPG signatures. It also makes the files immutable by mounting the filesystem as read-only, preventing unauthorized or accidental modifications.
- Flexibility: rpm-ostree allows you to layer additional RPM packages on top of the base image, or override existing packages with newer versions. You can also roll back to previous states of the system if something goes wrong. You can configure rpm-ostree with various settings for your device and server using .toml configuration files or command-line options.
- Compatibility: rpm-ostree is used by various Linux operating systems and tools, such as Fedora Silverblue, Fedora IoT, Fedora CoreOS, and Red Hat Enterprise Linux Atomic Host. It also supports different platforms, such as x86_64, ARMv7, and AArch64.

# OTA FOR ANDROID
There are two method for OTA update in Android.

- A/B (Seamless) System Update
- Non A/B System Update

## A/B (Seamless) System Update
Modern Android devices have two copies of each partition (A and B) and can apply an update to the currently unused partition while the system is running but idle. A/B devices do not need space to download the update package because they can apply the update as they read it from the network; this is known as streaming A/B.

### Workflow
SystemUpdaterSample app shows list of available updates on the UI. User is allowed to select an update and apply it to the device. App shows installation progress, logs can be found in adb logcat. User can stop or reset an update. Resetting the update requests update engine to cancel any ongoing update, and revert if the update has been applied. Stopping does not revert the applied update.

## Non A/B System Update
Older Android devices have a dedicated recovery partition containing the software needed to unpack a downloaded update package and apply the update to the other partitions.

## Life of an OTA Update in Android Devices
A typical OTA update contains the following steps:

- Device performs regular check in with OTA servers and is notified of the availability of an update, including the URL of the update package and a description string to show the user.
- Update downloads to a cache or data partition, and its cryptographic signature is verified against the certificates in /system/etc/security/otacerts.zip. User is prompted to install the update.
- Device reboots into recovery mode, in which the kernel and system in the recovery partition are booted instead of the kernel in the boot partition.
- Recovery binary is started by init. It finds command-line arguments in /cache/recovery/command that point it to the downloaded package.
- Recovery verifies the cryptographic signature of the package against the public keys in /res/keys (part of the RAM disk contained in the recovery partition)
- Data is pulled from the package and used to update the boot, system, and/or vendor partitions as necessary. One of the new files left on the system partition contains the contents of the new recovery partition.
- Device reboots normally.

a. The newly updated boot partition is loaded, and it mounts and starts executing binaries in the newly updated system partition.

b. As part of normal startup, the system checks the contents of the recovery partition against the desired contents (which were previously stored as a file in /system). They are different, so the recovery partition is reflashed with the desired contents. (On subsequent boots, the recovery partition already contains the new contents, so no reflash is necessary.)
- The system update is complete! The update logs can be found in /cache/recovery/last_log.#.
