# ----------------------------------------------------------------------------
# Makefile to compile and link stk500boot bootloader
# Author: Peter Fleury
# based on WinAVR Makefile Template written by Eric B. Weddington, J�rg Wunsch, et al.
#
# Adjust F_CPU below to the clock frequency in Mhz of your AVR target
# Adjust BOOTLOADER_ADDRESS to your AVR target
#
#----------------------------------------------------------------------------
# On command line:
#
# make all = Make software.
#
# make clean = Clean out built project files.
#
# make coff = Convert ELF to AVR COFF.
#
# make extcoff = Convert ELF to AVR Extended COFF.
#
# make program = Download the hex file to the device, using avrdude.
#                Please customize the avrdude settings below first!
#
# make debug = Start either simulavr or avarice as specified for debugging, 
#              with avr-gdb or avr-insight as the front end for debugging.
#
# make filename.s = Just compile filename.c into the assembler code only.
#
# make filename.i = Create a preprocessed source file for use in submitting
#                   bug reports to the GCC project.
#
# To rebuild project do "make clean" then "make all".
#----------------------------------------------------------------------------
#	<MLS> = Mark Sproul msproul-at-skychariot.com


# MCU name
#MCU = atmega128


# Processor frequency.
#     This will define a symbol, F_CPU, in all source code files equal to the 
#     processor frequency. You can then use this symbol in your source code to 
#     calculate timings. Do NOT tack on a 'UL' at the end, this will be done
#     automatically to create a 32-bit value in your source code.
#F_CPU = 16000000


# Bootloader
# Please adjust if using a different AVR
# 0x0e00*2=0x1C00 for ATmega8  512 words Boot Size
# 0xFC00*2=0x1F800 for ATmega128  1024 words Boot Size
# 0xF800*2=0x1F000 for ATmega1280
# 0xF000*2=0x1E000 for ATmega1280
#BOOTLOADER_ADDRESS = 1E000


# Output format. (can be srec, ihex, binary)
FORMAT = ihex


# Target file name (without extension).
TARGET = stk500boot


# List C source files here. (C dependencies are automatically generated.)
SRC = stk500boot.c 


# List Assembler source files here.
#     Make them always end in a capital .S.  Files ending in a lowercase .s
#     will not be considered source files but generated files (assembler
#     output from the compiler), and will be deleted upon "make clean"!
#     Even though the DOS/Win* filesystem matches both .s and .S the same,
#     it will preserve the spelling of the filenames, and gcc itself does
#     care about how the name is spelled on its command-line.
ASRC = 


# Optimization level, can be [0, 1, 2, 3, s]. 
#     0 = turn off optimization. s = optimize for size.
#     (Note: 3 is not always the best optimization level. See avr-libc FAQ.)
OPT = s


# Debugging format.
#     Native formats for AVR-GCC's -g are dwarf-2 [default] or stabs.
#     AVR Studio 4.10 requires dwarf-2.
#     AVR [Extended] COFF format requires stabs, plus an avr-objcopy run.
DEBUG = dwarf-2


# List any extra directories to look for include files here.
#     Each directory must be seperated by a space.
#     Use forward slashes for directory separators.
#     For a directory that has spaces, enclose it in quotes.
EXTRAINCDIRS = 


# Compiler flag to set the C Standard level.
#     c89   = "ANSI" C
#     gnu89 = c89 plus GCC extensions
#     c99   = ISO C99 standard (not yet fully implemented)
#     gnu99 = c99 plus GCC extensions
CSTANDARD = -std=gnu99


# Place -D or -U options here
CDEFS = -DF_CPU=$(F_CPU) -DBAUDRATE=$(BAUDRATE)


# Place -I options here
CINCS =



#---------------- Compiler Options ----------------
#  -g*:          generate debugging information
#  -O*:          optimization level
#  -f...:        tuning, see GCC manual and avr-libc documentation
#  -Wall...:     warning level
#  -Wa,...:      tell GCC to pass this to the assembler.
#    -adhlns...: create assembler listing
CFLAGS = -g$(DEBUG)
CFLAGS += $(CDEFS) $(CINCS)
CFLAGS += -O$(OPT)
CFLAGS += -funsigned-char -funsigned-bitfields -fpack-struct -fshort-enums -fno-jump-tables
CFLAGS += -Wall -Wstrict-prototypes
CFLAGS += -Wa,-adhlns=$(<:.c=.lst)
CFLAGS += $(patsubst %,-I%,$(EXTRAINCDIRS))
CFLAGS += $(CSTANDARD)


#---------------- Assembler Options ----------------
#  -Wa,...:   tell GCC to pass this to the assembler.
#  -ahlms:    create listing
#  -gstabs:   have the assembler create line number information; note that
#             for use in COFF files, additional information about filenames
#             and function names needs to be present in the assembler source
#             files -- see avr-libc docs [FIXME: not yet described there]
ASFLAGS = -Wa,-adhlns=$(<:.S=.lst),-gstabs 


#---------------- Library Options ----------------
# Minimalistic printf version
PRINTF_LIB_MIN = -Wl,-u,vfprintf -lprintf_min

# Floating point printf version (requires MATH_LIB = -lm below)
PRINTF_LIB_FLOAT = -Wl,-u,vfprintf -lprintf_flt

# If this is left blank, then it will use the Standard printf version.
PRINTF_LIB = 
#PRINTF_LIB = $(PRINTF_LIB_MIN)
#PRINTF_LIB = $(PRINTF_LIB_FLOAT)


# Minimalistic scanf version
SCANF_LIB_MIN = -Wl,-u,vfscanf -lscanf_min

# Floating point + %[ scanf version (requires MATH_LIB = -lm below)
SCANF_LIB_FLOAT = -Wl,-u,vfscanf -lscanf_flt

# If this is left blank, then it will use the Standard scanf version.
SCANF_LIB = 
#SCANF_LIB = $(SCANF_LIB_MIN)
#SCANF_LIB = $(SCANF_LIB_FLOAT)


MATH_LIB = -lm



#---------------- External Memory Options ----------------

# 64 KB of external RAM, starting after internal RAM (ATmega128!),
# used for variables (.data/.bss) and heap (malloc()).
#EXTMEMOPTS = -Wl,-Tdata=0x801100,--defsym=__heap_end=0x80ffff

# 64 KB of external RAM, starting after internal RAM (ATmega128!),
# only used for heap (malloc()).
#EXTMEMOPTS = -Wl,--defsym=__heap_start=0x801100,--defsym=__heap_end=0x80ffff

EXTMEMOPTS =




#---------------- Linker Options ----------------
#  -Wl,...:     tell GCC to pass this to linker.
#    -Map:      create map file
#    --cref:    add cross reference to  map file
LDFLAGS = -Wl,-Map=$(TARGET).map,--cref
LDFLAGS += $(EXTMEMOPTS)
LDFLAGS += $(PRINTF_LIB) $(SCANF_LIB) $(MATH_LIB)


#--------------- bootloader linker Options -------
# BOOTLOADER_ADDRESS (=Start of Boot Loader section
# in bytes - not words) is defined above.
#LDFLAGS += -Wl,--section-start=.text=$(BOOTLOADER_ADDRESS) -nostartfiles -nodefaultlibs
#LDFLAGS += -Wl,--section-start=.text=$(BOOTLOADER_ADDRESS) -nostartfiles
LDFLAGS += -Wl,--section-start=.text=$(BOOTLOADER_ADDRESS)

#---------------- Programming Options (avrdude) ----------------

# Programming hardware: alf avr910 avrisp bascom bsd 
# dt006 pavr picoweb pony-stk200 sp12 stk200 stk500
#
# Type: avrdude -c ?
# to get a full listing.
#
AVRDUDE_PROGRAMMER = stk500v2

# com1 = serial port. Use lpt1 to connect to parallel port.
AVRDUDE_PORT = com1    # programmer connected to serial device

AVRDUDE_WRITE_FLASH = -U flash:w:$(TARGET).hex
#AVRDUDE_WRITE_EEPROM = -U eeprom:w:$(TARGET).eep


# Uncomment the following if you want avrdude's erase cycle counter.
# Note that this counter needs to be initialized first using -Yn,
# see avrdude manual.
#AVRDUDE_ERASE_COUNTER = -y

# Uncomment the following if you do /not/ wish a verification to be
# performed after programming the device.
#AVRDUDE_NO_VERIFY = -V

# Increase verbosity level.  Please use this when submitting bug
# reports about avrdude. See <http://savannah.nongnu.org/projects/avrdude> 
# to submit bug reports.
#AVRDUDE_VERBOSE = -v -v

AVRDUDE_FLAGS = -p $(MCU) -P $(AVRDUDE_PORT) -c $(AVRDUDE_PROGRAMMER)
AVRDUDE_FLAGS += $(AVRDUDE_NO_VERIFY)
AVRDUDE_FLAGS += $(AVRDUDE_VERBOSE)
AVRDUDE_FLAGS += $(AVRDUDE_ERASE_COUNTER)



#---------------- Debugging Options ----------------

# For simulavr only - target MCU frequency.
DEBUG_MFREQ = $(F_CPU)

# Set the DEBUG_UI to either gdb or insight.
# DEBUG_UI = gdb
DEBUG_UI = insight

# Set the debugging back-end to either avarice, simulavr.
DEBUG_BACKEND = avarice
#DEBUG_BACKEND = simulavr

# GDB Init Filename.
GDBINIT_FILE = __avr_gdbinit

# When using avarice settings for the JTAG
JTAG_DEV = /dev/com1

# Debugging port used to communicate between GDB / avarice / simulavr.
DEBUG_PORT = 4242

# Debugging host used to communicate between GDB / avarice / simulavr, normally
#     just set to localhost unless doing some sort of crazy debugging when 
#     avarice is running on a different computer.
DEBUG_HOST = localhost



#============================================================================


# Define programs and commands.
SHELL = sh
TOOLROOT = /Applications/Arduino.app/Contents/Java/hardware/tools/avr/bin/
CC = $(TOOLROOT)avr-gcc
OBJCOPY = $(TOOLROOT)avr-objcopy
OBJDUMP = $(TOOLROOT)avr-objdump
SIZE = $(TOOLROOT)avr-size
NM = $(TOOLROOT)avr-nm
AVRDUDE = $(TOOLROOT)avrdude
REMOVE = rm -f
COPY = cp
WINSHELL = cmd


# Define Messages
# English
MSG_ERRORS_NONE = Errors: none
MSG_BEGIN = -------- begin --------
MSG_END = --------  end  --------
MSG_SIZE_BEFORE = Size before: 
MSG_SIZE_AFTER = Size after:
MSG_COFF = Converting to AVR COFF:
MSG_EXTENDED_COFF = Converting to AVR Extended COFF:
MSG_FLASH = Creating load file for Flash:
MSG_EEPROM = Creating load file for EEPROM:
MSG_EXTENDED_LISTING = Creating Extended Listing:
MSG_SYMBOL_TABLE = Creating Symbol Table:
MSG_LINKING = Linking:
MSG_COMPILING = Compiling:
MSG_ASSEMBLING = Assembling:
MSG_CLEANING = Cleaning project:

# Define all object files.
OBJ = $(SRC:.c=.o) $(ASRC:.S=.o) 

# Define all listing files.
LST = $(SRC:.c=.lst) $(ASRC:.S=.lst) 

# Compiler flags to generate dependency files.
GENDEPFLAGS = -MD -MP -MF .dep/$(@F).d

# Combine all necessary flags and optional flags.
# Add target processor to flags.
ALL_CFLAGS = -mmcu=$(MCU) -I. $(CFLAGS) $(GENDEPFLAGS)
ALL_ASFLAGS = -mmcu=$(MCU) -I. -x assembler-with-cpp $(ASFLAGS)


atmega2560_20mhz_115200:	MCU = atmega2560
atmega2560_20mhz_115200:	F_CPU = 20000000
atmega2560_20mhz_115200:	BAUDRATE = 115200
atmega2560_20mhz_115200:	BOOTLOADER_ADDRESS = 3E000
atmega2560_20mhz_115200:	CFLAGS += -D_MEGA_BOARD_
atmega2560_20mhz_115200:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_18_432mhz_115200:	MCU = atmega2560
atmega2560_18_432mhz_115200:	F_CPU = 18432000L
atmega2560_18_432mhz_115200:	BAUDRATE = 115200
atmega2560_18_432mhz_115200:	BOOTLOADER_ADDRESS = 3E000
atmega2560_18_432mhz_115200:	CFLAGS += -D_MEGA_BOARD_
atmega2560_18_432mhz_115200:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_16mhz_115200:	MCU = atmega2560
atmega2560_16mhz_115200:	F_CPU = 16000000L
atmega2560_16mhz_115200:	BAUDRATE = 115200
atmega2560_16mhz_115200:	BOOTLOADER_ADDRESS = 3E000
atmega2560_16mhz_115200:	CFLAGS += -D_MEGA_BOARD_
atmega2560_16mhz_115200:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_14_7456mhz_115200:	MCU = atmega2560
atmega2560_14_7456mhz_115200:	F_CPU = 14745600L
atmega2560_14_7456mhz_115200:	BAUDRATE = 115200
atmega2560_14_7456mhz_115200:	BOOTLOADER_ADDRESS = 3E000
atmega2560_14_7456mhz_115200:	CFLAGS += -D_MEGA_BOARD_
atmega2560_14_7456mhz_115200:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_12mhz_57600:	MCU = atmega2560
atmega2560_12mhz_57600:	F_CPU = 12000000L
atmega2560_12mhz_57600:	BAUDRATE = 57600
atmega2560_12mhz_57600:	BOOTLOADER_ADDRESS = 3E000
atmega2560_12mhz_57600:	CFLAGS += -D_MEGA_BOARD_
atmega2560_12mhz_57600:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_11_0592mhz_115200:	MCU = atmega2560
atmega2560_11_0592mhz_115200:	F_CPU = 11059200L
atmega2560_11_0592mhz_115200:	BAUDRATE = 115200
atmega2560_11_0592mhz_115200:	BOOTLOADER_ADDRESS = 3E000
atmega2560_11_0592mhz_115200:	CFLAGS += -D_MEGA_BOARD_
atmega2560_11_0592mhz_115200:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_8mhz_57600:	MCU = atmega2560
atmega2560_8mhz_57600:	F_CPU = 8000000L
atmega2560_8mhz_57600:	BAUDRATE = 57600
atmega2560_8mhz_57600:	BOOTLOADER_ADDRESS = 3E000
atmega2560_8mhz_57600:	CFLAGS += -D_MEGA_BOARD_
atmega2560_8mhz_57600:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_8mhz_38400:	MCU = atmega2560
atmega2560_8mhz_38400:	F_CPU = 8000000L
atmega2560_8mhz_38400:	BAUDRATE = 38400
atmega2560_8mhz_38400:	BOOTLOADER_ADDRESS = 3E000
atmega2560_8mhz_38400:	CFLAGS += -D_MEGA_BOARD_
atmega2560_8mhz_38400:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_8mhz_9600:	MCU = atmega2560
atmega2560_8mhz_9600:	F_CPU = 8000000L
atmega2560_8mhz_9600:	BAUDRATE = 9600
atmega2560_8mhz_9600:	BOOTLOADER_ADDRESS = 3E000
atmega2560_8mhz_9600:	CFLAGS += -D_MEGA_BOARD_
atmega2560_8mhz_9600:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_7_3728mhz_115200:	MCU = atmega2560
atmega2560_7_3728mhz_115200:	F_CPU = 7372800L
atmega2560_7_3728mhz_115200:	BAUDRATE = 115200
atmega2560_7_3728mhz_115200:	BOOTLOADER_ADDRESS = 3E000
atmega2560_7_3728mhz_115200:	CFLAGS += -D_MEGA_BOARD_
atmega2560_7_3728mhz_115200:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_4mhz_9600:	MCU = atmega2560
atmega2560_4mhz_9600:	F_CPU = 4000000L
atmega2560_4mhz_9600:	BAUDRATE = 9600
atmega2560_4mhz_9600:	BOOTLOADER_ADDRESS = 3E000
atmega2560_4mhz_9600:	CFLAGS += -D_MEGA_BOARD_
atmega2560_4mhz_9600:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_3_6864mhz_115200:	MCU = atmega2560
atmega2560_3_6864mhz_115200:	F_CPU = 3686400L
atmega2560_3_6864mhz_115200:	BAUDRATE = 115200
atmega2560_3_6864mhz_115200:	BOOTLOADER_ADDRESS = 3E000
atmega2560_3_6864mhz_115200:	CFLAGS += -D_MEGA_BOARD_
atmega2560_3_6864mhz_115200:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_2mhz_9600:	MCU = atmega2560
atmega2560_2mhz_9600:	F_CPU = 2000000L
atmega2560_2mhz_9600:	BAUDRATE = 9600
atmega2560_2mhz_9600:	BOOTLOADER_ADDRESS = 3E000
atmega2560_2mhz_9600:	CFLAGS += -D_MEGA_BOARD_
atmega2560_2mhz_9600:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_1_8432mhz_115200:	MCU = atmega2560
atmega2560_1_8432mhz_115200:	F_CPU = 1843200L
atmega2560_1_8432mhz_115200:	BAUDRATE = 115200
atmega2560_1_8432mhz_115200:	BOOTLOADER_ADDRESS = 3E000
atmega2560_1_8432mhz_115200:	CFLAGS += -D_MEGA_BOARD_
atmega2560_1_8432mhz_115200:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex

atmega2560_1mhz_9600:	MCU = atmega2560
atmega2560_1mhz_9600:	F_CPU = 1000000L
atmega2560_1mhz_9600:	BAUDRATE = 9600
atmega2560_1mhz_9600:	BOOTLOADER_ADDRESS = 3E000
atmega2560_1mhz_9600:	CFLAGS += -D_MEGA_BOARD_
atmega2560_1mhz_9600:	begin gccversion sizebefore build sizeafter end 
			mv $(TARGET).hex bootloaders/$(MCU)/stk500boot_v2_$(MCU)_$(BAUDRATE)_$(F_CPU).hex


build: elf hex eep lss sym
#build:  hex eep lss sym

elf: $(TARGET).elf
hex: $(TARGET).hex
eep: $(TARGET).eep
lss: $(TARGET).lss 
sym: $(TARGET).sym



# Eye candy.
# AVR Studio 3.x does not check make's exit code but relies on
# the following magic strings to be generated by the compile job.
begin:
	@echo
	@echo $(MSG_BEGIN)

end:
	@echo $(MSG_END)
	@echo


# Display size of file.
HEXSIZE = $(SIZE) --target=$(FORMAT) $(TARGET).hex
ELFSIZE = $(SIZE) --format=avr --mcu=$(MCU) $(TARGET).elf

sizebefore:
	@if test -f $(TARGET).elf; then echo; echo $(MSG_SIZE_BEFORE); $(ELFSIZE); \
	2>/dev/null; echo; fi

sizeafter:
	@if test -f $(TARGET).elf; then echo; echo $(MSG_SIZE_AFTER); $(ELFSIZE); \
	2>/dev/null; echo; fi



# Display compiler version information.
gccversion : 
	@$(CC) --version



# Program the device.  
program: $(TARGET).hex $(TARGET).eep
	$(AVRDUDE) $(AVRDUDE_FLAGS) $(AVRDUDE_WRITE_FLASH) $(AVRDUDE_WRITE_EEPROM)


# Generate avr-gdb config/init file which does the following:
#     define the reset signal, load the target file, connect to target, and set 
#     a breakpoint at main().
gdb-config: 
	@$(REMOVE) $(GDBINIT_FILE)
	@echo define reset >> $(GDBINIT_FILE)
	@echo SIGNAL SIGHUP >> $(GDBINIT_FILE)
	@echo end >> $(GDBINIT_FILE)
	@echo file $(TARGET).elf >> $(GDBINIT_FILE)
	@echo target remote $(DEBUG_HOST):$(DEBUG_PORT)  >> $(GDBINIT_FILE)
ifeq ($(DEBUG_BACKEND),simulavr)
	@echo load  >> $(GDBINIT_FILE)
endif	
	@echo break main >> $(GDBINIT_FILE)
	
debug: gdb-config $(TARGET).elf
ifeq ($(DEBUG_BACKEND), avarice)
	@echo Starting AVaRICE - Press enter when "waiting to connect" message displays.
	@$(WINSHELL) /c start avarice --jtag $(JTAG_DEV) --erase --program --file \
	$(TARGET).elf $(DEBUG_HOST):$(DEBUG_PORT)
	@$(WINSHELL) /c pause
	
else
	@$(WINSHELL) /c start simulavr --gdbserver --device $(MCU) --clock-freq \
	$(DEBUG_MFREQ) --port $(DEBUG_PORT)
endif
	@$(WINSHELL) /c start avr-$(DEBUG_UI) --command=$(GDBINIT_FILE)
	



# Convert ELF to COFF for use in debugging / simulating in AVR Studio or VMLAB.
COFFCONVERT=$(OBJCOPY) --debugging \
--change-section-address .data-0x800000 \
--change-section-address .bss-0x800000 \
--change-section-address .noinit-0x800000 \
--change-section-address .eeprom-0x810000 



coff: $(TARGET).elf
	@echo
	@echo $(MSG_COFF) $(TARGET).cof
	$(COFFCONVERT) -O coff-avr $< $(TARGET).cof


extcoff: $(TARGET).elf
	@echo
	@echo $(MSG_EXTENDED_COFF) $(TARGET).cof
	$(COFFCONVERT) -O coff-ext-avr $< $(TARGET).cof


# Create final output files (.hex, .eep) from ELF output file.
%.hex: %.elf
	@echo
	@echo $(MSG_FLASH) $@
	$(OBJCOPY) -O $(FORMAT) -R .eeprom $< $@

%.eep: %.elf
	@echo
	@echo $(MSG_EEPROM) $@
	-$(OBJCOPY) -j .eeprom --set-section-flags=.eeprom="alloc,load" \
	--change-section-lma .eeprom=0 -O $(FORMAT) $< $@

# Create extended listing file from ELF output file.
%.lss: %.elf
	@echo
	@echo $(MSG_EXTENDED_LISTING) $@
	$(OBJDUMP) -h -S $< > $@

# Create a symbol table from ELF output file.
%.sym: %.elf
	@echo
	@echo $(MSG_SYMBOL_TABLE) $@
	$(NM) -n $< > $@



# Link: create ELF output file from object files.
.SECONDARY : $(TARGET).elf
.PRECIOUS : $(OBJ)
%.elf: $(OBJ)
	@echo
	@echo $(MSG_LINKING) $@
	$(CC) $(ALL_CFLAGS) $^ --output $@ $(LDFLAGS)


# Compile: create object files from C source files.
%.o : %.c
	@echo
	@echo $(MSG_COMPILING) $<
	$(CC) -c $(ALL_CFLAGS) $< -o $@ 


# Compile: create assembler files from C source files.
%.s : %.c
	$(CC) -S $(ALL_CFLAGS) $< -o $@


# Assemble: create object files from assembler source files.
%.o : %.S
	@echo
	@echo $(MSG_ASSEMBLING) $<
	$(CC) -c $(ALL_ASFLAGS) $< -o $@

# Create preprocessed source for use in sending a bug report.
%.i : %.c
	$(CC) -E -mmcu=$(MCU) -I. $(CFLAGS) $< -o $@ 


# Target: clean project.
clean: begin clean_list end

clean_list :
	@echo
	@echo $(MSG_CLEANING)
	$(REMOVE) *.hex
	$(REMOVE) *.eep
	$(REMOVE) *.cof
	$(REMOVE) *.elf
	$(REMOVE) *.map
	$(REMOVE) *.sym
	$(REMOVE) *.lss
	$(REMOVE) $(OBJ)
	$(REMOVE) $(LST)
	$(REMOVE) $(SRC:.c=.s)
	$(REMOVE) $(SRC:.c=.d)
	rm -rf .dep



# Include the dependency files.
-include $(shell mkdir .dep 2>/dev/null) $(wildcard .dep/*)


# Listing of phony targets.
.PHONY : all begin finish end sizebefore sizeafter gccversion \
build elf hex eep lss sym coff extcoff \
clean clean_list program debug gdb-config

