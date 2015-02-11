# mbed_rtos_blinky
This is an attempt to get a very simple app to compile using the latest mbed and mbed-rtos libraries compiled with gcc-arm-none-eabi

## Dev Environment
- Mac OS X 10.10
- gcc version 4.9.3 20141119 (release) [ARM/embedded-4_9-branch revision 218278] (GNU Tools for ARM Embedded Processors)

## Current Error

```
make clean && make
Removing entire out directory
rm -rf mbed.elf mbed.bin mbed.map build/*.* build/

Building file: HAL_CM.o
Invoking: MCU C Compiler
arm-none-eabi-gcc -g -ggdb -Os -Wall -fno-strict-aliasing -fno-rtti -ffunction-sections -fdata-sections -fno-exceptions -fno-delete-null-pointer-checks -fmessage-length=0 -fno-builtin -mthumb -mcpu=cortex-m3 -MMD -MP -DTARGET_LPC1768 -DTOOLCHAIN_GCC_ARM -DNDEBUG -MF build/HAL_CM.d -I.  -Imbed  -Imbed/TARGET_LPC1768  -Imbed/TARGET_LPC1768/TOOLCHAIN_GCC_ARM  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X/TARGET_MBED_LPC1768  -Irtos  -Irtx/TARGET_CORTEX_M  -Irtx/TARGET_CORTEX_M/TARGET_M3/TOOLCHAIN_GCC -std=gnu99 -c rtx/TARGET_CORTEX_M/HAL_CM.c -o build/HAL_CM.o
cc1: warning: command line option '-fno-rtti' is valid for C++/ObjC++ but not for C
Finished building: HAL_CM.o

Building file: RTX_Conf_CM.o
Invoking: MCU C Compiler
arm-none-eabi-gcc -g -ggdb -Os -Wall -fno-strict-aliasing -fno-rtti -ffunction-sections -fdata-sections -fno-exceptions -fno-delete-null-pointer-checks -fmessage-length=0 -fno-builtin -mthumb -mcpu=cortex-m3 -MMD -MP -DTARGET_LPC1768 -DTOOLCHAIN_GCC_ARM -DNDEBUG -MF build/RTX_Conf_CM.d -I.  -Imbed  -Imbed/TARGET_LPC1768  -Imbed/TARGET_LPC1768/TOOLCHAIN_GCC_ARM  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X/TARGET_MBED_LPC1768  -Irtos  -Irtx/TARGET_CORTEX_M  -Irtx/TARGET_CORTEX_M/TARGET_M3/TOOLCHAIN_GCC -std=gnu99 -c rtx/TARGET_CORTEX_M/RTX_Conf_CM.c -o build/RTX_Conf_CM.o
cc1: warning: command line option '-fno-rtti' is valid for C++/ObjC++ but not for C
In file included from rtx/TARGET_CORTEX_M/RTX_Conf_CM.c:35:0:
rtx/TARGET_CORTEX_M/RTX_Conf_CM.c:93:25: error: 'WORDS_STACK_SIZE' undeclared here (not in a function)
  #define OS_TIMERSTKSZ  WORDS_STACK_SIZE
                         ^
rtx/TARGET_CORTEX_M/cmsis_os.h:340:38: note: in definition of macro 'osThreadDef'
 uint32_t os_thread_def_stack_##name [stacksz / sizeof(uint32_t)]; \
                                      ^
rtx/TARGET_CORTEX_M/RTX_CM_lib.h:120:60: note: in expansion of macro 'OS_TIMERSTKSZ'
 osThreadDef(osTimerThread, (osPriority)(OS_TIMERPRIO-3), 4*OS_TIMERSTKSZ);
                                                            ^
make: *** [build/RTX_Conf_CM.o] Error 1
```
