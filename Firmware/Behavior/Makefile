PRJ_NAME = Behavior
CORE = ATxmega128A1U
MCU = atxmega128a1u
CORE_VERSION = 1.15

# Files and directories
BUILD_DIR = bin
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

all: $(BUILD_DIR)/$(PRJ_NAME)-harp$(CORE_VERSION).hex

$(BUILD_DIR):
	mkdir "$(BUILD_DIR)"

$(BUILD_DIR)/%.o: %.c $(BUILD_DIR)
	$(CC) $(CFLAGS) -c $(INCS) -MD -MP -MF "$(@:.o=.d)" -MT"$(@:.o=.d)" -MT"$@" -o $@ $<

$(BUILD_DIR)/%.elf: $(OBJS)
	$(CC) -o $@ $(OBJS) -Wl,-Map="$(@:.elf=.map)" $(LDFLAGS) $(LIBS)

$(BUILD_DIR)/%.hex: $(BUILD_DIR)/%.elf
	$(OBJCOPY) $(OBJCOPY_FLAGS) $< $@
