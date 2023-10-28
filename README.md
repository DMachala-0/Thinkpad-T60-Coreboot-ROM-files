# Thinkpad-T60-ROM-files
Collection of stock BIOS files for thinkpad T60 series with ATI GPUs and Coreboot configs.


# T60(x1400 128MB)
Able to get usable ram to 3.5GB with modification of Coreboot code.

Dont know what will fail/override that memory space.

Change:

  -> src/northbridge/intel/i945/raminit.c -> DEFAULT_PCI_MMIO_SIZE -> 512
  
  -> src/mainboard/lenovo/t60/devicetree.cb -> register "pci_mmio_size" = "512"

# T60p(v5250 256MB)
Unable to get more ram from MMIO (fail to boot)

Result:
  -> 3.2GB only

# VGABIOS extract 
ATI GPUs need init ROM.

If LCD is changed and resolution is diffrent, new VGABIOS is needed.

Reason for this is when seabios gives control to OS, OS is unable to change correctly LVDS channel size (seabios is able) between single or dual.

Results in running display incorrectly.

Solution to this is switching to original BIOS (with no devices that would display whitelist error) and get VGABIOS from OS and reconfigure coreboot with it.


Useful command, use with stock bios, when reconfigured VGA BIOS is needed for diffrent LCD display.

linux (bibanon wiki):

cat /proc/iomem | grep 'Video ROM' | (read m; m=${m/ :*}; s=${m/-*}; e=${m/*-}; \
dd if=/dev/mem of=vgabios.bin bs=1c skip=$[0x$s] count=$[$[0x$e]-$[0x$s]+1])

# Programmer
stm32-vserprog:
Useful programmer for SPI memory

# Also:

Problem with sata connection lockup after long/random IO ops (this is due to linux changes in drivers , kernel 5.xx+ affected, but 4.3,4.19 unaffected).

If possible add extension wires from ROM to memory compartment for easier and quicker programming.

ref:
https://github.com/dword1511/stm32-vserprog
https://github.com/bibanon/Coreboot-ThinkPads/wiki/ThinkPad-T60p
