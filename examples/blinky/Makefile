CC = xtensa-lx106-elf-gcc
CFLAGS = -I./inc -mlongcalls
LDLIBS = -nostdlib -Wl,--start-group -lmain -lnet80211 -lwpa -llwip -lpp -lphy -lc -Wl,--end-group -lgcc
LDFLAGS = -Teagle.app.v6.ld

SDK = ~/repo/esp-open-sdk/sdk
ESPTOOL = esptool.py
DEV = /dev/ttyS3

TARGET = blinky

.PHONY: clean flash erase

SRC = $(wildcard *.c)
OBJ = $(SRC:.c=.o)

${TARGET}.elf-0x?0000.bin: ${TARGET}.elf
	${ESPTOOL} elf2image $^

${TARGET}.elf: ${OBJ}
	${CC} ${LDLIBS} ${LDFLAGS} $^ -o $@

%.o: %.c
	$(CC) $(CFLAGS) -c $<

install: flash

flash: ${TARGET}.elf-0x?0000.bin
	${ESPTOOL} -p ${DEV} write_flash 0x00000 ${TARGET}-0x00000.bin 0x10000 ${TARGET}-0x10000.bin

erase: 
	${ESPTOOL} -p ${DEV} erase_flash	
	${ESPTOOL} -p ${DEV} write_flash 0x3fc000 ${SDK}/bin/esp_init_data_default.bin

clean:
	rm -fv ${OBJ} ${TARGET}.elf $(TARGET).elf-0x?0000.bin
