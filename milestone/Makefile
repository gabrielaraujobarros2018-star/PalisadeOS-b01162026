AS = nasm
CC = clang
LD = ld.lld

CFLAGS = -ffreestanding -fno-stack-protector -fno-pic -O2 -Wall -Wextra -Iinclude

all: image

# ---------------- BIOS ----------------

stage1.bin:
	$(AS) -I bios/ bios/stage1.asm -f bin -o stage1.bin

stage2.o:
	$(AS) bios/stage2.asm -f elf32 -o stage2.o

bios: stage1.bin stage2.o
	$(LD) -m elf_i386 -T build/bios.ld stage2.o -o stage2.elf

# ---------------- IMAGE ----------------

image: bios
	dd if=/dev/zero of=bootloader.img bs=512 count=4096
	dd if=stage1.bin of=bootloader.img conv=notrunc
	dd if=stage2.elf of=bootloader.img seek=1 conv=notrunc

# ---------------- CLEAN ----------------

clean:
	rm -f stage1.bin stage2.o stage2.elf bootloader.img