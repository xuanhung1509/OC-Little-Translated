# ACPI Basics
- [A brief Introduction to ACPI](#a-brief-introduction-to-acpi)
  - [What is ACPI?](#what-is-acpi)
  - [Why to prefer SSDTs over a patched DSDT](#why-to-prefer-ssdts-over-a-patched-dsdt)

## A brief Introduction to ACPI
"ACPI… the final frontier…". No, not really. But this is how overwhelmed most Hackintosh users feel the first time, they open an `.aml` file and look inside it. Understanding what to make of all of it seems like an expedition of epochal proportions impossible to grasp. And that's who this introduction is for.

Did you ever wonder what a `DSDT` or `SSDT` table is and what it does? Or how these rename patches that you have in your `config.plist` work? Well, after reading this, you will know for sure! But first things first…

### What is ACPI?
**ACPI** stands for **Advanced Configuration & Power Interface**. It is an architecture-independent power management and configuration standard originally developed by Intel, Microsoft, Toshiba and other manufacturers who formed the ACPI special interest group. In October 2013, the assets of the ACPI specifications were transferred to the UEFI Forum. The latest version of the ACPI Specification (v6.4) was released in January 2021.

In the ACPI, peripheral devices and system hardware features of the platform are described in the **`DSDT`** (Differentiated System Description Table), which is loaded at boot and in SSDTs (Secondary System Description Tables), which are loaded *dynamically* at run time. The ACPI Form describes a machine's hardware information in `.aml` format and does not have any driver capabilities of its own. ACPI is literally just a set of tables of texts (converted into binary code) to provide operating systems with some basic information about the used hardware. **`DSDTs`** and **`SSDTs`** are just *two* of many tables that make up a system's ACPI – but very important ones for us.

Each computer mainboard ships with a set of ACPI Tables stored in the BIOS (or UEFI). The number and content of ACPI tables varies from used mainboard and chipset - it even *can* vary between different BIOS versions as well.

ACPI serves as an interface layer between the operating system and system firmware, as shown below:

![](https://uefi.org/specs/ACPI/6.4/_images/acpi-overview.png)</br>
**Source**: [**UEFI.org**](https://uefi.org/specs/ACPI/6.4/Frontmatter/Overview/Overview.html)

The ACPI subsystem itself consists of 2 data structures: data tables and definition blocks (see figure below). These data structures are the primary communication mechanism between the firmware and the OS. Data tables store raw data and are consumed by device drivers. Definition blocks consist of byte code that is executable by an interpreter.

![](https://uefi.org/specs/ACPI/6.4/_images/acpi-structure.png)

**Source**: [**UEFI.org**](https://uefi.org/specs/ACPI/6.4/Frontmatter/Overview/Overview.html)

We can make use of SSDTs to inject said Definintion Blocks into the system to change things as needed. We can add (virtual) devices, rename devices, change control methods (or redefine them), modify buffers, etc.) so macOS is happy.

### Why to prefer SSDTs over a patched DSDT
A common problem with Hackintoshes is missing ACPI functionality when trying to run macOS on X86-based Intel and AMD systems, such as: Networking not working, USB Ports not working, CPU Power Management not working correctly, screens not turning off when the lid is closed, Sleep and Wake not working, Brightness controls not working and so on.

These issues stem from DSDTs made with Windows support in mind on one hand and Apple not sticking to ACPI tables which conform to the ACPI specs 100 % for their hardware on the other. These issues can be addressed by dumping, patching and injecting a patched DSDT during boot, replacing the original.

Since a DSDT can change when updating the BIOS, injecting an older DSDT on top of a newer one can cause conflicts and break macOS functionalities. Therefore, *dynamic patching* with SSDTs is highly recommended over using a patched DSDT. Plus the whole process is much more efficient, transparent and elegant. And that's why you should avoid patched DSDTs.

Continue to the next Chapter, [**ASL Basics**](https://github.com/5T33Z0/OC-Little-Translated/blob/main/00_ACPI/ACPI_Basics/ASL_Basics.md).

## CREDITS and RESOURCES
- Original [**ASL Guide**](https://bbs.pcbeta.com/forum.php?mod=viewthread&tid=944566&archive=2&extra=page%3D1&page=1) by suhetao
- ACPI [**Specifications**](https://uefi.org/htmlspecs/ACPI_Spec_6_4_html/)
- ASL Tutorial by acpica.org ([**PDF**](https://acpica.org/sites/acpica/files/asl_tutorial_v20190625.pdf)). Good starting point if you want to get into fixing your `DSDT` with `SSDT` hotpatches.