# Diskpart: How to format drives

Open *command prompt* and enter diskpart. A new shell opens.

```cmd
DISKPART> list disk
DISKPART> select disk 2
DISKPART> clean
DISKPART> create partition primary
DISKPART> select partition 1
DISKPART> active
DISKPART> format fs=ntfs label=USB
DISKPART> assign letter=w
DISKPART> exit
```
