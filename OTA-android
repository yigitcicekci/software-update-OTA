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
