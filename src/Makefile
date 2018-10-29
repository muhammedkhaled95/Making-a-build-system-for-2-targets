#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#     <FILE>.o : builds <FILE>.o object file for specific source file
#     <FILE>.i : builds <FILE>.i preprocessed file for specific source file
#     <FILE>.asm : generates the assembly file for a specific source file and
#     the assembly code for the final executable file using the objdump utility
#     compile-all : compile all object files but don't link
#     build: builds and links all the source files
#     clean: clean all the generated files
#     all: same as build
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM = HOST
#general flags for both platforms
GENERALFLAGS = -Wall -Werror -g -O0 -std=c99
TARGET = c1m2

ifeq ($(PLATFORM),HOST)
   CC = gcc
   LD = ld
# compiler, linker, preprocessor flags
   CFLAGS = $(GENERALFLAGS)
   LDFLAGS = -Wl,-Map=$(TARGET).map 
endif

ifeq ($(PLATFORM),MSP432)
   CC = arm-none-eabi-gcc
   LD = arm-none-eabi-ld
# Architectures Specific Flags
   LINKER_FILE = msp432p401r.lds 
   CPU = cortex-m4 
   ARCH = thumb
   ARM = armv7e-m
   SPECS = nosys.specs
   FLOAT = hard
   FPU = fpv4-sp-d16
# compiler, linker, and preprocessor flags
   CFLAGS = -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) -march=$(ARM) -mfloat-abi=$(FLOAT) -mfpu=$(FPU) $(GENERALFLAGS)
   LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)  
endif


OBJECTS := $(SOURCES:.c=.o)


#RULES for build targets

%.i : %.c 
	$(CC) -D$(PLATFORM) -E $^ -o $@ 

%.o : %.c
	$(CC) $(CFLAGS) -D$(PLATFORM) -O $@ -c $^

%.asm : %.c
	$(CC) $(CFLAGS) -D$(PLATFORM) -S $^

.PHONY:  compile-all
compile-all: $(OBJECTS)
$(OBJECTS): $(SOURCES)
	$(CC) -D$(PLATFORM) $(CFLAGS) -c $^

.PHONY: build
build: $(TARGET).out
$(TARGET).out: $(OBJECTS)
	$(CC) -D$(PLATFORM) $(CFLAGS) $(LDFLAGS) $^ -o  $@

.PHONY: clean
clean:
	rm *.o $(TARGET).map *.d *.i *.s $(TARGET).out

