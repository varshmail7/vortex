RISCV_TOOLCHAIN_PATH = /opt/riscv-vec

RISCV_PREFIX ?= riscv32-unknown-elf-
VORTEX_RT_PATH ?= $(realpath ../../../runtime)

CC = $(RISCV_TOOLCHAIN_PATH)/bin/$(RISCV_PREFIX)gcc
AR = $(RISCV_TOOLCHAIN_PATH)/bin/$(RISCV_PREFIX)gcc-ar
DP = $(RISCV_TOOLCHAIN_PATH)/bin/$(RISCV_PREFIX)objdump
CP = $(RISCV_TOOLCHAIN_PATH)/bin/$(RISCV_PREFIX)objcopy

SIM_DIR=../../../sim

CFLAGS += -march=rv32imfv -mabi=ilp32f

CFLAGS += -O3 -Wstack-usage=1024 -mcmodel=medany -ffreestanding -nostartfiles -fdata-sections -ffunction-sections
CFLAGS += -I$(VORTEX_RT_PATH)/include -I$(VORTEX_RT_PATH)/../hw

LDFLAGS += -lm -Wl,-Bstatic,-T,$(VORTEX_RT_PATH)/linker/vx_link32.ld -Wl,--gc-sections $(VORTEX_RT_PATH)/libvortexrt.a

PROJECT = vecadd

SRCS = main.cpp

all: $(PROJECT).elf $(PROJECT).bin $(PROJECT).dump

$(PROJECT).dump: $(PROJECT).elf
	$(DP) -D $(PROJECT).elf > $(PROJECT).dump

$(PROJECT).bin: $(PROJECT).elf
	$(CP) -O binary $(PROJECT).elf $(PROJECT).bin

$(PROJECT).elf: $(SRCS)
	$(CC) $(CFLAGS) $(SRCS) $(LDFLAGS) -o $(PROJECT).elf

run-rtlsim: $(PROJECT).bin
	$(SIM_DIR)/rtlsim/rtlsim $(PROJECT).bin

run-simx: $(PROJECT).bin
	$(SIM_DIR)/simx/simx -c 1 -i $(PROJECT).bin

.depend: $(SRCS)
	$(CC) $(CFLAGS) -MM $^ > .depend;

clean:
	rm -rf *.elf *.bin *.dump .depend 
