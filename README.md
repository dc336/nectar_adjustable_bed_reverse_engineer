# Hacking a bed frame
// Notes


https://www.nectarsleep.com/bed-frames/adjustable-bed-frame

Driving Chip: Nordic Semiconductor nRF52810

<img width="475" height="480" alt="image" src="https://github.com/user-attachments/assets/b228b748-16c8-4a65-aeb5-7f09da99b1fa" />

32-bit Arm Cortex-M4 64MHz

192 KB Flash / 24KB Ram

1-2 MBps -- BLE / ANT / Proprietary 2.4ghz

1.7-3.6v

GPIO, SPI, I2C, UART, PWM, ADC, AES


Initial Entry:

SWD Debug pins easily accessible. Protection is disabled so core is unlocked and so is dumping:

openocd -f interface/cmsis-dap.cfg -c "transport select swd" -f target/nrf52.cfg -c 'adapter speed 1000; init; halt 3000; dump_image firmware1.bin 0x00000000 0x30000; verify_image firmware1.bin 0x00000000 bin; resume; shutdown'

Get offsets (ChatGPT):

python3 -c 'import struct; b=open("firmware1.bin","rb").read(8); sp,rv=struct.unpack("<II",b); print(f"Initial SP: 0x{sp:08x}\nReset vector: 0x{rv:08x}\nCode address: 0x{rv & ~1:08x}")'

Initial SP: 0x200019f0
Reset vector: 0x000002dd
Code address: 0x000002dc



String dump:

<img width="769" height="530" alt="image" src="https://github.com/user-attachments/assets/9af80be5-d0d2-4e85-a73a-b7e2447bafda" />
