# Making a UEFI bootable iPXE USB drive

## Build the UEFI executable for iPXE

```bash
# First we'll clone iPXE
$ git clone git://git.ipxe.org/ipxe.git
# Go into the src directory of the cloned git repo
$ cd ipxe/src
# Compile the UEFI iPXE executable
$ make bin-x86_64-efi/ipxe.efi
```

## Format the USB drive

```bash
# First we'll wipe the USB drive (THIS WILL DESTROY ALL DATA)
$ sudo dd if=/dev/zero of=/dev/sdxY bs=512 count=1
# Then we'll partition the drive
$ sudo cfdisk /dev/sdxY
# cfdisk will ask you what type of partition table you want, select GPT
```

### Partitioning the USB drive

1. Make a partition at least ``512M`` in size.
2. Switch the type from ``Linux Filesystem`` to ``EFI system``.
3. Write these changes to the USB drive and quit.

## Formatting, and mounting the USB drive

```bash
# First we'll format it FAT32
$ sudo mkfs.fat -F32 /dev/sdxY
# Now we'll make a directory to mount the USB drive in
$ mkdir /tmp/efidrive
# Now we can mount the USB drive
$ sudo mount /dev/sdxY /tmp/efidrive
```

### Placing the UEFI iPXE executable

```bash
# Assuming you're still in the ipxe/src directory
# Make the necessary efi/boot directory in the USB drive
$ sudo mkdir -p /tmp/efidrive/efi/boot
# copy the executable and rename it to bootx64.efi to conform to the UEFI standard
$ sudo cp bin-x86_64-efi/ipxe.efi /tmp/efidrive/efi/boot/bootx64.efi
# unmount the drive
$ sudo umount /tmp/efidrive
```

## You did it!

That's it! You should now have a UEFI-bootable USB drive with the UEFI ipxe binary!

## Credit
* [@AdrianKoshka](https://gist.github.com/AdrianKoshka/5b6f8b6803092d8b108cda2f8034539a)
* [arch wiki ESP page](https://wiki.archlinux.org/index.php/EFI_System_Partition#Format_the_partition)
* [iPXE download page](http://ipxe.org/download)
