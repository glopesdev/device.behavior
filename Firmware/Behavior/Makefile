PRJ_NAME = Behavior
CORE = ATxmega128A1U
MCU = atxmega128a1u
CORE_VERSION = 1.15
FIRMWARE_VERSION ?= 3.3
HARDWARE_VERSION ?= 2.0

# Files and directories
BUILD_DIR = bin
PREVIEW_SUFFIX = $(if $(PREVIEW_VERSION),-preview$(PREVIEW_VERSION),)
BUILD_NAME = $(PRJ_NAME)-fw$(FIRMWARE_VERSION)-harp$(CORE_VERSION)-hw$(HARDWARE_VERSION)-ass0$(PREVIEW_SUFFIX)
FW_VERSION_WORDS = $(subst ., ,$(FIRMWARE_VERSION))
HW_VERSION_WORDS = $(subst ., ,$(HARDWARE_VERSION))
SRCS = $(wildcard *.c)
OBJS = $(SRCS:%.c=$(BUILD_DIR)/%.o)
LIBS = -l$(CORE)-$(CORE_VERSION)

# Compiler flags
CC = avr-gcc
CFLAGS = \
-x c \
-funsigned-char \
-funsigned-bitfields \
-ffunction-sections \
-fdata-sections \
-fpack-struct \
-fshort-enums \
-mrelax \
-O3 \
-Wall \
-mmcu=$(MCU) \
-DMAJOR_FW_VERSION=$(word 1,$(FW_VERSION_WORDS)) \
-DMINOR_FW_VERSION=$(word 2,$(FW_VERSION_WORDS)) \
-DMAJOR_HW_VERSION=$(word 1,$(HW_VERSION_WORDS)) \
-DMINOR_HW_VERSION=$(word 2,$(HW_VERSION_WORDS)) \
-std=gnu99

# Linker flags
LDFLAGS = \
-Wl,--start-group \
-Wl,-lm \
-Wl,$(LIBS) \
-Wl,--end-group \
-Wl,-L"." \
-Wl,--gc-sections \
-mrelax \
-mmcu=$(MCU)

# Object copy flags
OBJCOPY = avr-objcopy
OBJCOPY_FLAGS = \
-O ihex \
-R .eeprom \
-R .fuse \
-R .lock \
-R .signature \
-R .user_signatures \

all: $(BUILD_DIR)/$(BUILD_NAME).hex

clean:
	rm -f $(OBJS) $(OBJS:.o=.d)
	rm -f $(BUILD_DIR)/$(BUILD_NAME).elf
	rm -f $(BUILD_DIR)/$(BUILD_NAME).hex
	rm -f $(BUILD_DIR)/$(BUILD_NAME).map

$(BUILD_DIR):
	mkdir "$(BUILD_DIR)"

$(BUILD_DIR)/%.o: %.c $(BUILD_DIR)
	$(CC) $(CFLAGS) -c $(INCS) -MD -MP -MF "$(@:.o=.d)" -MT"$(@:.o=.d)" -MT"$@" -o $@ $<

$(BUILD_DIR)/%.elf: $(OBJS)
	$(CC) -o $@ $(OBJS) -Wl,-Map="$(@:.elf=.map)" $(LDFLAGS) $(LIBS)

$(BUILD_DIR)/%.hex: $(BUILD_DIR)/%.elf
	$(OBJCOPY) $(OBJCOPY_FLAGS) $< $@
