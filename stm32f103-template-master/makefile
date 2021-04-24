# simple ARM Makefile
#
# written by Ahmed Badra (http://github.com/badra022)
# released to the public domain

# Makefile
#
# targets:
#   all:    compiles the source code
#   clean:  removes all .hex, .elf, and .o files in the source code and library directories

# parameters (change this stuff accordingly)
# project name
PRJ = main
# avr mcu
CPU = cortex-m3
# program source files (not including external libraries)
SRC = $(PRJ).c
# where to look for external libraries (consisting of .c/.cpp files and .h files)
# e.g. EXT = ../../EyeToSee ../../YouSART
EXT = .././STM32F10x_StdPeriph_Lib_V3.5.0
EXT += .././STM32F10x_StdPeriph_Lib_V3.5.0/CMSIS/Device/ST/STM32F10x/Include
EXT += .././STM32F10x_StdPeriph_Lib_V3.5.0/CMSIS/Include
EXT += .././STM32F10x_StdPeriph_Lib_V3.5.0/Libraries/inc
EXT += .././STM32F10x_StdPeriph_Lib_V3.5.0/Libraries/src

# include path
INCLUDE := $(foreach dir, $(EXT), -I$(dir))
# c flags
CFLAGS  = -g -O2 -Wall $(INCLUDE)
CFLAGS += -T../STM32F10x_StdPeriph_Lib_V3.5.0/stm32_flash.ld
CFLAGS += -mlittle-endian -mthumb -mcpu=$(CPU) -mthumb-interwork -std=gnu11
CFLAGS += -DSTM32F10X_MD -DUSE_STDPERIPH_DRIVER
CFLAGS += -Wl,--gc-sections

# executables
OBJCOPY 	= .././ArmGCC/bin/arm-none-eabi-objcopy.exe
CC				= .././ArmGCC/bin/arm-none-eabi-gcc.exe
# OBJCOPY = arm-none-eabi-objcopy
# CC      = arm-none-eabi-gcc

# generate list of objects
CFILES    = $(filter %.c, $(SRC))
EXTC     := $(foreach dir, $(EXT), $(wildcard $(dir)/*.c))
EXTS 		 := $(foreach dir, $(EXT), $(wildcard $(dir)/*.s))
OBJ       = $(CFILES:.c=.o) $(EXTC:.c=.o) $(EXTS:.c=.o)

# user targets
# compile all files
all: $(PRJ).hex

# remove compiled files
clean:
	rm -f main.hex main.elf main.o main.bin
	# $(foreach dir, $(EXT), rm -f $(dir)/*.o;)

# other targets
# objects from c files
.c.o:
	$(CC) $(CFLAGS) -c $< -o $@

# elf file
$(PRJ).elf: $(OBJ)
	$(CC) $(CFLAGS) -o $(PRJ).elf $(OBJ)

# hex file
$(PRJ).hex: $(PRJ).elf
	rm -f $(PRJ).hex
	$(OBJCOPY) -O ihex $(PRJ).elf $(PRJ).hex
	$(OBJCOPY) -O binary $(PRJ).elf $(PRJ).bin
