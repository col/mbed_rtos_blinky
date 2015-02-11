# mbed_rtos_blinky
This is an attempt to get a very simple app to compile using the latest mbed and mbed-rtos libraries compiled with gcc-arm-none-eabi to run of the LPC1768.

## Dev Environment
- Mac OS X 10.10
- gcc version 4.9.3 20141119 (release) [ARM/embedded-4_9-branch revision 218278] (GNU Tools for ARM Embedded Processors)

## First Compile Error

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

This error stems from this piece of code in cmis_os.h

```
// The stack space occupied is mainly dependent on the underling C standard library
#if defined(TOOLCHAIN_GCC) || defined(TOOLCHAIN_ARM_STD) || defined(TOOLCHAIN_IAR)
#    define WORDS_STACK_SIZE   512
#elif defined(TOOLCHAIN_ARM_MICRO)
#    define WORDS_STACK_SIZE   128
#endif
```

It's a hack but I decided to add the toolchain I have defined in my Makefile to get this to work.

```
// The stack space occupied is mainly dependent on the underling C standard library
#if defined(TOOLCHAIN_GCC) || defined(TOOLCHAIN_ARM_STD) || defined(TOOLCHAIN_IAR) || defined(TOOLCHAIN_GCC_ARM)
#    define WORDS_STACK_SIZE   512
#elif defined(TOOLCHAIN_ARM_MICRO)
#    define WORDS_STACK_SIZE   128
#endif
```

It's not idea but it works for now.

## Second Compile Error

```
make clean && make
Removing entire out directory
rm -rf mbed.elf mbed.bin mbed.map build/*.* build/

Building file: HAL_CM.o
Invoking: MCU C Compiler
arm-none-eabi-gcc -g -ggdb -Os -Wall -fno-strict-aliasing -fno-rtti -ffunction-sections -fdata-sections -fno-exceptions -fno-delete-null-pointer-checks -fmessage-length=0 -fno-builtin -mthumb -mcpu=cortex-m3 -MMD -MP -DTARGET_LPC1768 -DTOOLCHAIN_GCC_ARM -DNDEBUG  -MF build/HAL_CM.d -I.  -Imbed  -Imbed/TARGET_LPC1768  -Imbed/TARGET_LPC1768/TOOLCHAIN_GCC_ARM  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X/TARGET_MBED_LPC1768  -Irtos  -Irtx/TARGET_CORTEX_M  -Irtx/TARGET_CORTEX_M/TARGET_M3/TOOLCHAIN_GCC -std=gnu99 -c rtx/TARGET_CORTEX_M/HAL_CM.c -o build/HAL_CM.o
cc1: warning: command line option '-fno-rtti' is valid for C++/ObjC++ but not for C
Finished building: HAL_CM.o

Building file: RTX_Conf_CM.o
Invoking: MCU C Compiler
arm-none-eabi-gcc -g -ggdb -Os -Wall -fno-strict-aliasing -fno-rtti -ffunction-sections -fdata-sections -fno-exceptions -fno-delete-null-pointer-checks -fmessage-length=0 -fno-builtin -mthumb -mcpu=cortex-m3 -MMD -MP -DTARGET_LPC1768 -DTOOLCHAIN_GCC_ARM -DNDEBUG  -MF build/RTX_Conf_CM.d -I.  -Imbed  -Imbed/TARGET_LPC1768  -Imbed/TARGET_LPC1768/TOOLCHAIN_GCC_ARM  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X/TARGET_MBED_LPC1768  -Irtos  -Irtx/TARGET_CORTEX_M  -Irtx/TARGET_CORTEX_M/TARGET_M3/TOOLCHAIN_GCC -std=gnu99 -c rtx/TARGET_CORTEX_M/RTX_Conf_CM.c -o build/RTX_Conf_CM.o
cc1: warning: command line option '-fno-rtti' is valid for C++/ObjC++ but not for C
Finished building: RTX_Conf_CM.o

Building file: rt_CMSIS.o
Invoking: MCU C Compiler
arm-none-eabi-gcc -g -ggdb -Os -Wall -fno-strict-aliasing -fno-rtti -ffunction-sections -fdata-sections -fno-exceptions -fno-delete-null-pointer-checks -fmessage-length=0 -fno-builtin -mthumb -mcpu=cortex-m3 -MMD -MP -DTARGET_LPC1768 -DTOOLCHAIN_GCC_ARM -DNDEBUG  -MF build/rt_CMSIS.d -I.  -Imbed  -Imbed/TARGET_LPC1768  -Imbed/TARGET_LPC1768/TOOLCHAIN_GCC_ARM  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X  -Imbed/TARGET_LPC1768/TARGET_NXP/TARGET_LPC176X/TARGET_MBED_LPC1768  -Irtos  -Irtx/TARGET_CORTEX_M  -Irtx/TARGET_CORTEX_M/TARGET_M3/TOOLCHAIN_GCC -std=gnu99 -c rtx/TARGET_CORTEX_M/rt_CMSIS.c -o build/rt_CMSIS.o
cc1: warning: command line option '-fno-rtti' is valid for C++/ObjC++ but not for C
rtx/TARGET_CORTEX_M/rt_CMSIS.c:46:4: error: #error "Missing __CORTEX_Mx definition"
   #error "Missing __CORTEX_Mx definition"
    ^
In file included from rtx/TARGET_CORTEX_M/rt_CMSIS.c:60:0:
rtx/TARGET_CORTEX_M/rt_HAL_CM.h: In function 'rt_inc_qi':
rtx/TARGET_CORTEX_M/rt_HAL_CM.h:212:3: warning: implicit declaration of function '__disable_irq' [-Wimplicit-function-declaration]
   __disable_irq();
   ^
rtx/TARGET_CORTEX_M/rt_HAL_CM.h:219:3: warning: implicit declaration of function '__enable_irq' [-Wimplicit-function-declaration]
   __enable_irq ();
   ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: In function 'svcKernelStart':
rtx/TARGET_CORTEX_M/rt_CMSIS.c:447:3: warning: implicit declaration of function '__set_PSP' [-Wimplicit-function-declaration]
   __set_PSP(os_tsk.run->tsk_stack + 8*4);       // New context
   ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: In function 'osKernelInitialize':
rtx/TARGET_CORTEX_M/rt_CMSIS.c:466:3: warning: implicit declaration of function '__get_IPSR' [-Wimplicit-function-declaration]
   if (__get_IPSR() != 0) return osErrorISR;     // Not allowed in ISR
   ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:467:3: warning: implicit declaration of function '__get_CONTROL' [-Wimplicit-function-declaration]
   if ((__get_CONTROL() & 1) == 0) {             // Privileged mode
   ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: In function 'osKernelStart':
rtx/TARGET_CORTEX_M/rt_CMSIS.c:483:9: warning: implicit declaration of function '__set_CONTROL' [-Wimplicit-function-declaration]
         __set_CONTROL(0x02);                    // Set Privileged Thread mode & PSP
         ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:487:7: warning: implicit declaration of function '__DSB' [-Wimplicit-function-declaration]
       __DSB();
       ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:488:7: warning: implicit declaration of function '__ISB' [-Wimplicit-function-declaration]
       __ISB();
       ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: At top level:
rtx/TARGET_CORTEX_M/rt_CMSIS.c:958:8: error: unknown type name '__INLINE'
 static __INLINE osStatus isrMessagePut (osMessageQId queue_id, uint32_t info, uint32_t millisec);
        ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:958:26: error: expected '=', ',', ';', 'asm' or '__attribute__' before 'isrMessagePut'
 static __INLINE osStatus isrMessagePut (osMessageQId queue_id, uint32_t info, uint32_t millisec);
                          ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: In function 'sysTimerTick':
rtx/TARGET_CORTEX_M/rt_CMSIS.c:972:5: warning: implicit declaration of function 'isrMessagePut' [-Wimplicit-function-declaration]
     isrMessagePut(osMessageQId_osTimerMessageQ, (uint32_t)pt, 0);
     ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: At top level:
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1122:8: error: unknown type name '__INLINE'
 static __INLINE int32_t isrSignalSet (osThreadId thread_id, int32_t signals) {
        ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1122:25: error: expected '=', ',', ';', 'asm' or '__attribute__' before 'isrSignalSet'
 static __INLINE int32_t isrSignalSet (osThreadId thread_id, int32_t signals) {
                         ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: In function 'osSignalSet':
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1144:5: warning: implicit declaration of function 'isrSignalSet' [-Wimplicit-function-declaration]
     return   isrSignalSet(thread_id, signals);
     ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: At top level:
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1383:8: error: unknown type name '__INLINE'
 static __INLINE osStatus isrSemaphoreRelease (osSemaphoreId semaphore_id) {
        ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1383:26: error: expected '=', ',', ';', 'asm' or '__attribute__' before 'isrSemaphoreRelease'
 static __INLINE osStatus isrSemaphoreRelease (osSemaphoreId semaphore_id) {
                          ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: In function 'osSemaphoreRelease':
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1421:5: warning: implicit declaration of function 'isrSemaphoreRelease' [-Wimplicit-function-declaration]
     return   isrSemaphoreRelease(semaphore_id);
     ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: At top level:
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1621:8: error: unknown type name '__INLINE'
 static __INLINE osStatus isrMessagePut (osMessageQId queue_id, uint32_t info, uint32_t millisec) {
        ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1621:26: error: expected '=', ',', ';', 'asm' or '__attribute__' before 'isrMessagePut'
 static __INLINE osStatus isrMessagePut (osMessageQId queue_id, uint32_t info, uint32_t millisec) {
                          ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1639:8: error: unknown type name '__INLINE'
 static __INLINE os_InRegs osEvent isrMessageGet (osMessageQId queue_id, uint32_t millisec) {
        ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1639:35: error: expected '=', ',', ';', 'asm' or '__attribute__' before 'isrMessageGet'
 static __INLINE os_InRegs osEvent isrMessageGet (osMessageQId queue_id, uint32_t millisec) {
                                   ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c: In function 'osMessageGet':
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1691:5: warning: implicit declaration of function 'isrMessageGet' [-Wimplicit-function-declaration]
     return   isrMessageGet(queue_id, millisec);
     ^
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1691:5: error: incompatible types when returning type 'int' but 'osEvent' was expected
rtx/TARGET_CORTEX_M/rt_CMSIS.c:1695:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
make: *** [build/rt_CMSIS.o] Error 1
```

This error comes from here in rt_CMSIS.c

```
#if defined (__CORTEX_M4) || defined (__CORTEX_M4F)
  #include "core_cm4.h"
#elif defined (__CORTEX_M3)
  #include "core_cm3.h"
#elif defined (__CORTEX_M0)
  #include "core_cm0.h"
#elif defined (__CORTEX_M0PLUS)
  #include "core_cm0plus.h"
#else
  #error "Missing __CORTEX_Mx definition"
#endif
```

I'm only trying to get this to run on an LPC1768 so I simply added the ```__CORTEX_M3``` to the CC_SYMBOLS in the Makefile. It worked!
This is probably a better approach for fixing the original compile error as well so I added ```TOOLCHAIN_GCC``` and removed my changes from ```cmsis_os.h```.

```
CC_SYMBOLS = -D$(TARGET_BOARD) -DTOOLCHAIN_GCC_ARM -DNDEBUG -D__CORTEX_M3 -DTOOLCHAIN_GCC
```

Compile Successful!!

Having both ```TOOLCHAIN_GCC_ARM``` and ```TOOLCHAIN_GCC``` defined maybe isn't the best solution but it'll do for now.
