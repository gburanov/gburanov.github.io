---
layout: post
title: "UEFI - welcome note"
modified:
categories: blog
excerpt:
tags: [UEFI, bios, C++]
image:
  feature:
date: 2010-06-05T15:39:55
---
# UEFI – welcome note

We will cover some topic here on what is UEFI (former EFI) and how to deal with it. We will cover topic, dealing with bootability, OS loading, kernel loading and so on. The question, what is EFI, will be discussed later, so I am assuming you are currently already familiar with basic concepts. So, let’s just start working.

There are three stages of UEFI implementation in the motherboard.

Stage 1 –  EFI(also known as UEFI version 1), is implemented on Apple computers. That’s how MacOS is loading, and that one of the reasons, why installing Windows on Apple computer involves installing special loader, BOOTCAMP (we are not currently talking about different filesystems for Windows and MacOs, HFS and NTFS). EFI is 32-bit. We will cover EFI later.

Stage 2 is UEFI mixed with BIOS (old way of loading). 99% or UEFI computer are now backwards compatible with BIOS for the simple reason – many OSes currently lack good UEFI support. Generally, UEFI stage 2 is 64-bit (although theoretically 32-bit UEFI is possible). In stage 2 computers one can enable UEFI by checking checkbox in BIOS (see picture). Now, almost all 64-bit Intel motherboards, and some others as well support Stage 2.

![]({{ site.url }}/images/blog/2010-06-05-uefi-welcome-note/enable_uefi_in_bios.jpg)

Stage 3 computers do not have backward compatibility with BIOS, so only OSes that support UEFI can be installed on the computer. Stage 3 computers are quite rare now, and I have not see one, although Intel claim they have one.

Now, we came to the topic “What OSes currently have UEFI support”. Windows 7 64-bit and Windows Vista 64-bit have quite good UEFI support, and it really works. Talking about Linux, RedHat Linux and Fedora (starting from version 11) claim that they support UEFI, but this is now not stable, mostly because of the problems with UEFI versions of grub and lilo (again, we will talk about it later in details).

Let’s try install Windows 7 on UEFI Stage 2 computer. Tick the UEFI support in BIOS (as shown in picture above), restart your computer and press F10, when asked. You will notice now boot menu and you need to choose item with text UEFI and CD/DVD (name differs in different implementations). And, here we go, installation starts. Before Windows loads it’s own video drivers, image can have problems, because current versions of UEFI video drivers are quite bad.

![]({{ site.url }}/images/blog/2010-06-05-uefi-welcome-note/start_boot_menu.jpg)

![]({{ site.url }}/images/blog/2010-06-05-uefi-welcome-note/boot_menu.jpg)

Let’s now see what we got “under the cover”. First, open the Disk Manager and notice that the disk is GPT, not MBR, and that we got additional EFI system partition on the drive. EFI System partition is the special partition where all EFI first-stage loader are located. It has special GUID, and that’s how UEFI locates it

![]({{ site.url }}/images/blog/2010-06-05-uefi-welcome-note/efi_system_partition.jpg)

Normally, Windows does not mount this partition on with drive letter, because it is internal, but you can force it to be mounted using mountvol command with special syntax. mountvol letter: /s. Let’s try calling this command under “Administrator”

```
mountvol Z: /s
```

(assuming that we don’t have Z: drive yet) and than browse it, using, for example Total Commander (also under Administrator priviledge).

We now notice, that this partition containg EFI version of Windows Boot Manager. We will talk about it in the next topic.

![]({{ site.url }}/images/blog/2010-06-05-uefi-welcome-note/browsed_efi_system_partitio.jpg)

If we browse GPT disk using diskpart, we will notice, that the disk has one more partition, that is now shown in Disk Manager. This is Microsoft Reserved Partition and Microsoft creates it on every GPT drive, even if it is not bootable. Microsoft uses this partition to convert from basic disk to dynamic on GPT, because both GPT and LDM(dynamic disks) store their metadata at the end of the drive (opposite to MBR disks, that contains metadata in the beginning of the disk). This partition is not very important for us, because we will not covert dynamic disks on GPT right now.

```
DISKPART> select disk 2

Disk 2 is now the selected disk.

DISKPART> list partition

  Partition ###  Type              Size     Offset
  -------------  ----------------  -------  -------
  Partition 1    Reserved           128 MB    17 KB
  Partition 2    System             100 MB   129 MB
  Partition 3    Primary            146 GB   229 MB
```
