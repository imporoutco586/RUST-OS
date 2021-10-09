target := riscv32-riscv_os
mode := debug
kernel := target/$(target)/$(mode)/riscv_os
bin := target/$(target)/$(mode)/kernel.bin
usr_path := usr

export SFSIMG = $(usr_path)/rcore32.img

.PHONY: all clean run build asm qemu kernel

all: build

build: $(bin)

run: build qemu

kernel:
	@cargo xbuild	--target	riscv32-riscv_os.json

$(bin): kernel
	@riscv64-unknown-elf-objcopy $(kernel) --strip-all -O binary $@

asm:
	@riscv64-unknown-elf-objdump -d $(kernel) | less

qemu:
	@qemu-system-riscv32 -nographic -machine virt \
        -kernel opensbi/virt.elf \
        -device loader,file=$(bin),addr=0x80400000