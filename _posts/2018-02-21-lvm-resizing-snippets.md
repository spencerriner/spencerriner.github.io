---
layout: post
title: "LVM Resizing Snippets"
date: 2018-02-21
tags: [guide, linux, lvm]
category: sysadmin
comments: true
share: true
---

I can never remember how to expand a filesystem if I'm expanding an LVM volume, so I'm writing it down for future reference.

I also ran into an interesting situation recently that I thought might be worth documenting. We use [kickstart](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/sect-kickstart-howto) to provision common operating systems on new computers, and recently got two HP Z800s to use as CI builders. They came loaded with a 3.5" hard disk drive and an M.2 NVMe drive. While installing Scientific Linux 6, we ran into some issues where the OS basically wouldn't load at all and skipped over the boot disk, no matter what partitioning or LVM scheme we used. I suspected that the NVMe drive was the culprit and I was right - after removing the drive, the install completed fine. However, this meant that I had two existing volumes (`/home` and `swap`) that needed to be moved to the fast drive. Took me a few tries, but eventually I figured it out.

<!--description-->

I'll start easy with expanding an existing filesystem.

## Extend a LVM Logical Volume

This is for CentOS or Scientific Linux 6, which use `ext4` filesystems by default. This actually makes a difference later.

### Identify how much free space is available on the PV

`pvs`

`pvs` is basically a shorthand command for `pvdisplay`, though the output is less verbose. `vgs` and `lvs` do the same thing for their respective expanded commands. After performing this command, if any space is left over after creating a volume group, you can see how much you have to work with. 

### Extend LV

`lvextend -L+50G /dev/mapper/vg_sl6-lv_root`

Specify the volume here at the end.

### Resize filesystem on partition

`resize2fs /dev/mapper/vg_sl6-lv_root`

The `resize2fs` command figures out how much space the filesystem has to expand and goes for it all automatically. There is an option to specify the size, but you really don't need to use it in this case. 

## CentOS 7 Bonus Round

If you use `resize2fs` on CentOS 7, you'll probably see a message that looks something like this:

```
resize2fs: Bad magic number in super-block while trying to open /dev/mapper/centos-root
Couldn't find valid filesystem superblock.
```

That's because CentOS 7 uses **xfs** filesystems by default and has its own command.

`xfs_growfs /dev/mapper/vg_c7-lv_root`

The more you know! Now here's how to switch volumes from one disk to another, including `swap` which has a tricky pitfall you should avoid.

## Move Existing LVM Volumes from one VG to Another

### Create Physical Volume on new disk

`lvmdiskscan`

This command shows disk and partition data, even for disks that don't have any LVM. You could also probably find the drive in `dmesg` or a similar tool. My drive is at `/dev/nvme0n1`. Remember that in this case, the NVMe drive has no data and I'm okay with losing anything that may be on there. Take backups before messing around with LVM if you're really worried about data integrity! Make a new physical volume:

`pvcreate /dev/nvme0n1`

You'll get an error/warning if there's already stuff here. Proceed with caution.

### Create new Volume Group

`vgcreate vg_nvme /dev/nvme0n1`

### Remove existing partitions to be moved to new disk

Again - *this will delete stuff*. I doubt anyone besides myself is using this as a guide, but on the off chance that someone is and then gets mad at me, be smart about this. 

```bash
# /home
umount /home
lvremove /dev/mapper/vg_os-lv_home

# swap
swapoff -v /dev/mapper/vg_os-lv_swap
lvremove /dev/mapper/vg_os-lv_swap
```

### Create new volumes

```bash
# /home
lvcreate -L 800G -n lv_home vg_nvme
mkfs.ext4 /dev/mapper/vg_nvme-lv_home

# swap
lvcreate -L 50G -n lv_swap vg_nvme
mkswap /dev/mapper/vg_nvme-lv_swap
```

### Edit config files

You then must edit `/etc/fstab` to change the mount locations. Change whatever volume group the `/home` and `swap` volumes were in from the old one to the new one.

`swapon -va`

Turn swap back on and make sure you can see it.

`cat /proc/swaps`
`free`

Here's the part that got me the first time I did this: **you have to change the grub config or you will kernel panic on boot**.

And then all you can really do is boot into a Live CD or start this entire process over, so just do this before rebooting.

Edit `/boot/grub/grub.cfg` and change the kernel lines to reflect the new VG name. It should say something like `rd_LVM_LV=vg_nvme/lv_swap`. 

### Mount new filesystems and test with mount, df

`mount -a`

You should see your new, hyper-fast, expanding `/home` directory. We already checked on swap, but feel free to throw another `free` in the mix. You can now reboot and enjoy those sweet 3GB/s speeds.

## Closing Thoughts

I like LVM, even though it was really complicated and confusing to me at first. Honestly, anything involving partitioning was confusing to me, but after a lot of repetition and trial/error, I think I pretty much get it. LVM is nice because it's flexible - you can expand volumes as needed, which means that you can have a standard template with minimal sizes defined and modify later based on the needs of the individual system (disk space, etc.). However, failures can be harder to recover from. I don't have much experience with this, but I spent about 3 days trying to recover lost data from an LVM system and it continues to haunt me. Luckily, there are LVM backup files in `/etc/lvm/backup` that saved my skin. Either way, options are nice to have. Until next time!
