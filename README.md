
# linux-device-driver-1

Repository of Linux device driver programming(LDD1) Udemy course

## Table of content

[Section 1. Host and Target Setup](#section-1)

[Additional Refernces](#additional-refernces)

## Section 1

### Host Setup and Refernce Documents

1. Install on the host packages from [required.txt](./required.txt)
2. Download toolchain:
   * Download latest image from [official site](https://beagleboard.org/latest-images)
     * **TODO**: Ubuntu image [Tutorial on elinux web-site](https://elinux.org/BeagleBoardUbuntu)

   * Download cross-compiler from:
     * [Linaro Toolchain Binaries](https://releases.linaro.org/components/toolchain/binaries/)
       * [used gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf](https://releases.linaro.org/components/toolchain/binaries/7.5-2019.12/arm-linux-gnueabihf/)
     * **TODO**: Look for other toolchains
       * [ARMv7 Fork](https://github.com/artemmiesianinov/armv7-multiplatform)
       * [TI kernel customized script](https://github.com/artemmiesianinov/ti-linux-kernel-dev)

3. Add toolchain in PATH:
   `export PATH=$PATH:<path_to_tool_chain_binaries>`

   **TODO**: create git submodule which downloaded compiler binaries

4. The reference documents
   * [AM335x Technical reference manual](https://www.ti.com/lit/ug/spruh73q/spruh73q.pdf)
   * [AM335x Datasheet](https://www.ti.com/lit/ds/symlink/am3358.pdf?ts=1673861612770&ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252FAM3358)
   * [BeagleBone Black System Reference Manual](https://cdn-shop.adafruit.com/datasheets/BBB_SRM.pdf)
   * [Device tree specification document](https://github.com/devicetree-org/devicetree-specification/releases/download/v0.3/devicetree-specification-v0.3.pdf)

### Preparing microSD card

1. The size of SD card can be 8/16GBs
2. Run `sudo gparted`
3. You can double check the name for the partition of SD card with `sudo dmesg`
4. Create two partitions on SD card:
   * BOOT with fat16 (256 - 512 MB)
   * ROOTFS ext4 (the remaning space)
5. Set flags for BOOT partition:
   * boot
   * lba
6. Look for path where SD card's partitions mounted with `lsblk`
7. Copy files with [pre-build images](./downloads/pre-built-images/) into BOOT partition
8. Mount Debian image and `cd to_image_mount point`. And copy all from rootfs to mounted SD card's ROOTFS:

   `sudo cp -a * /media/$USER/ROOOTFS`
9. Unmount SD card's volumes and eject card from reader.
10. Insert SD card into BBB
11. Optional. Connect serial-to-ttl usb dongle (use `sudo dmesg` for detect mount point for dongle)
12. Launch minicom with `sudo minicom -s` (-s option allows to ommit the "/dev/modem not found" error)
    * Make config:
      * A: `/dev/ttyUSB<X>`
      * F: Hardware Flow control No
      * Save as default
13. Power on BBB holding S2 button
14. Remove boot image from eMMC to make the SD card will be default without S2 button pressing
    * Backup MBR using following command: `dd if=/dev/mmcblk1 of=emmcboot.img bs=1M count=1`
    * Remove MBR using `dd if=/dev/zero of=/dev/mmcblk1 bs=1M count=1`

**TODO**: Recover eMMC and describe here. Check how image at eMMC updated via official docs

## Additional Refernces

**TODO**: [PRU cookbook](https://beagleboard.org/static/prucookbook/)
