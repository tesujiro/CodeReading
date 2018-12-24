# Repository
Go compiler for small devices, based on LLVM.
https://github.com/aykevl/tinygo

# Run
command:
- build
- flash,gdb
- run
- clean
- help

# Example program (blinky):
```
import (
	"machine"
	"time"
)

func main() {
	led := machine.GPIO{machine.LED}
	led.Configure(machine.GPIOConfig{Mode: machine.GPIO_OUTPUT})
	for {
		led.Low()
		time.Sleep(time.Millisecond * 1000)

		led.High()
		time.Sleep(time.Millisecond * 1000)
	}
}
```
# Basic code path 1 : main - 

# Files
```
$ tree -h -P "\*.go"
.
├── [1.1K]  go-llvm
└── [ 992]  tinygo
    ├── [  96]  bin
    ├── [ 352]  compiler
    ├── [ 448]  docs
    ├── [ 288]  interp
    ├── [ 160]  ir
    ├── [ 224]  lib
    │   ├── [ 224]  CMSIS
    │   │   ├── [ 480]  CMSIS
    │   │   │   ├── [ 128]  DAP
    │   │   │   │   └── [ 224]  Firmware
    │   │   │   │       ├── [  96]  Config
    │   │   │   │       ├── [ 128]  Examples
    │   │   │   │       │   ├── [ 384]  LPC-Link-II
    │   │   │   │       │   │   ├── [  96]  Objects
    │   │   │   │       │   │   └── [ 192]  RTE
    │   │   │   │       │   │       ├── [  96]  CMSIS
    │   │   │   │       │   │       ├── [  96]  Device
    │   │   │   │       │   │       │   └── [ 160]  LPC4320_Cortex-M4
    │   │   │   │       │   │       └── [ 128]  USB
    │   │   │   │       │   └── [ 416]  XMC4200
    │   │   │   │       │       ├── [  96]  Objects
    │   │   │   │       │       └── [ 192]  RTE
    │   │   │   │       │           ├── [  96]  CMSIS
    │   │   │   │       │           ├── [  96]  Device
    │   │   │   │       │           │   └── [ 160]  XMC4200-Q48x256
    │   │   │   │       │           └── [ 128]  USB
    │   │   │   │       ├── [  96]  Include
    │   │   │   │       ├── [ 224]  Source
    │   │   │   │       └── [  96]  Template
    │   │   │   │           └── [ 160]  MDK5
    │   │   │   ├── [ 160]  DSP_Lib
    │   │   │   │   ├── [ 416]  Examples
    │   │   │   │   │   ├── [ 128]  arm_class_marks_example
    │   │   │   │   │   │   ├── [ 256]  ARM
    │   │   │   │   │   │   │   └── [ 128]  RTE
    │   │   │   │   │   │   │       └── [ 192]  Device
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM0
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM3
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │   │           └── [ 128]  ARMCM7_SP
    │   │   │   │   │   │   └── [ 256]  GCC
    │   │   │   │   │   │       └── [ 288]  Startup
    │   │   │   │   │   ├── [ 128]  arm_convolution_example
    │   │   │   │   │   │   ├── [ 320]  ARM
    │   │   │   │   │   │   │   └── [ 128]  RTE
    │   │   │   │   │   │   │       └── [ 192]  Device
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM0
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM3
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │   │           └── [ 128]  ARMCM7_SP
    │   │   │   │   │   │   └── [ 320]  GCC
    │   │   │   │   │   │       └── [ 288]  Startup
    │   │   │   │   │   ├── [ 128]  arm_dotproduct_example
    │   │   │   │   │   │   ├── [ 256]  ARM
    │   │   │   │   │   │   │   └── [ 128]  RTE
    │   │   │   │   │   │   │       └── [ 192]  Device
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM0
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM3
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │   │           └── [ 128]  ARMCM7_SP
    │   │   │   │   │   │   └── [ 256]  GCC
    │   │   │   │   │   │       └── [ 288]  Startup
    │   │   │   │   │   ├── [ 128]  arm_fft_bin_example
    │   │   │   │   │   │   ├── [ 288]  ARM
    │   │   │   │   │   │   │   └── [ 128]  RTE
    │   │   │   │   │   │   │       └── [ 192]  Device
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM0
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM3
    │   │   │   │   │   │   │           ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │   │           └── [ 128]  ARMCM7_SP
    │   │   │   │   │   │   └── [ 288]  GCC
    │   │   │   │   │   │       └── [ 288]  Startup
    │   │   │   │   │   ├── [  96]  arm_fir_example
    │   │   │   │   │   │   └── [ 352]  ARM
    │   │   │   │   │   │       └── [ 128]  RTE
    │   │   │   │   │   │           └── [ 192]  Device
    │   │   │   │   │   │               ├── [ 128]  ARMCM0
    │   │   │   │   │   │               ├── [ 128]  ARMCM3
    │   │   │   │   │   │               ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │               └── [ 128]  ARMCM7_SP
    │   │   │   │   │   ├── [  96]  arm_graphic_equalizer_example
    │   │   │   │   │   │   └── [ 352]  ARM
    │   │   │   │   │   │       └── [ 128]  RTE
    │   │   │   │   │   │           └── [ 192]  Device
    │   │   │   │   │   │               ├── [ 128]  ARMCM0
    │   │   │   │   │   │               ├── [ 128]  ARMCM3
    │   │   │   │   │   │               ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │               └── [ 128]  ARMCM7_SP
    │   │   │   │   │   ├── [  96]  arm_linear_interp_example
    │   │   │   │   │   │   └── [ 352]  ARM
    │   │   │   │   │   │       └── [ 128]  RTE
    │   │   │   │   │   │           └── [ 192]  Device
    │   │   │   │   │   │               ├── [ 128]  ARMCM0
    │   │   │   │   │   │               ├── [ 128]  ARMCM3
    │   │   │   │   │   │               ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │               └── [ 128]  ARMCM7_SP
    │   │   │   │   │   ├── [  96]  arm_matrix_example
    │   │   │   │   │   │   └── [ 320]  ARM
    │   │   │   │   │   │       └── [ 128]  RTE
    │   │   │   │   │   │           └── [ 192]  Device
    │   │   │   │   │   │               ├── [ 128]  ARMCM0
    │   │   │   │   │   │               ├── [ 128]  ARMCM3
    │   │   │   │   │   │               ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │               └── [ 128]  ARMCM7_SP
    │   │   │   │   │   ├── [  96]  arm_signal_converge_example
    │   │   │   │   │   │   └── [ 352]  ARM
    │   │   │   │   │   │       └── [ 128]  RTE
    │   │   │   │   │   │           └── [ 192]  Device
    │   │   │   │   │   │               ├── [ 128]  ARMCM0
    │   │   │   │   │   │               ├── [ 128]  ARMCM3
    │   │   │   │   │   │               ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │               └── [ 128]  ARMCM7_SP
    │   │   │   │   │   ├── [  96]  arm_sin_cos_example
    │   │   │   │   │   │   └── [ 256]  ARM
    │   │   │   │   │   │       └── [ 128]  RTE
    │   │   │   │   │   │           └── [ 192]  Device
    │   │   │   │   │   │               ├── [ 128]  ARMCM0
    │   │   │   │   │   │               ├── [ 128]  ARMCM3
    │   │   │   │   │   │               ├── [ 128]  ARMCM4_FP
    │   │   │   │   │   │               └── [ 128]  ARMCM7_SP
    │   │   │   │   │   └── [  96]  arm_variance_example
    │   │   │   │   │       └── [ 256]  ARM
    │   │   │   │   │           └── [ 128]  RTE
    │   │   │   │   │               └── [ 192]  Device
    │   │   │   │   │                   ├── [ 128]  ARMCM0
    │   │   │   │   │                   ├── [ 128]  ARMCM3
    │   │   │   │   │                   ├── [ 128]  ARMCM4_FP
    │   │   │   │   │                   └── [ 128]  ARMCM7_SP
    │   │   │   │   └── [ 448]  Source
    │   │   │   │       ├── [ 192]  ARM
    │   │   │   │       ├── [1.2K]  BasicMathFunctions
    │   │   │   │       ├── [ 128]  CommonTables
    │   │   │   │       ├── [ 640]  ComplexMathFunctions
    │   │   │   │       ├── [ 320]  ControllerFunctions
    │   │   │   │       ├── [ 320]  FastMathFunctions
    │   │   │   │       ├── [3.2K]  FilteringFunctions
    │   │   │   │       ├── [ 192]  GCC
    │   │   │   │       ├── [ 864]  MatrixFunctions
    │   │   │   │       ├── [ 864]  StatisticsFunctions
    │   │   │   │       ├── [ 704]  SupportFunctions
    │   │   │   │       └── [1.1K]  TransformFunctions
    │   │   │   ├── [ 384]  Documentation
    │   │   │   │   ├── [  96]  Core
    │   │   │   │   │   └── [4.1K]  html
    │   │   │   │   │       └── [5.8K]  search
    │   │   │   │   ├── [  96]  DAP
    │   │   │   │   │   └── [3.8K]  html
    │   │   │   │   │       └── [2.1K]  search
    │   │   │   │   ├── [  96]  DSP
    │   │   │   │   │   └── [ 32K]  html
    │   │   │   │   │       └── [5.9K]  search
    │   │   │   │   ├── [  96]  Driver
    │   │   │   │   │   └── [ 13K]  html
    │   │   │   │   │       └── [4.2K]  search
    │   │   │   │   ├── [  96]  General
    │   │   │   │   │   └── [1.5K]  html
    │   │   │   │   ├── [  96]  Pack
    │   │   │   │   │   └── [3.5K]  html
    │   │   │   │   │       └── [1.4K]  search
    │   │   │   │   ├── [  96]  RTOS
    │   │   │   │   │   └── [3.8K]  html
    │   │   │   │   ├── [ 128]  RTX
    │   │   │   │   │   └── [4.4K]  html
    │   │   │   │   │       └── [3.3K]  search
    │   │   │   │   └── [  96]  SVD
    │   │   │   │       └── [2.7K]  html
    │   │   │   ├── [ 128]  Driver
    │   │   │   │   ├── [ 416]  DriverTemplates
    │   │   │   │   └── [ 544]  Include
    │   │   │   ├── [ 576]  Include
    │   │   │   ├── [ 160]  Lib
    │   │   │   │   ├── [ 512]  ARM
    │   │   │   │   └── [ 288]  GCC
    │   │   │   ├── [ 128]  Pack
    │   │   │   │   ├── [ 320]  Example
    │   │   │   │   │   ├── [  96]  Boards
    │   │   │   │   │   │   └── [  96]  Keil
    │   │   │   │   │   │       └── [  96]  MCB1800
    │   │   │   │   │   │           └── [ 288]  RTX_Blinky
    │   │   │   │   │   │               └── [ 160]  RTE
    │   │   │   │   │   │                   ├── [  96]  CMSIS
    │   │   │   │   │   │                   └── [  96]  Device
    │   │   │   │   │   │                       └── [ 160]  LPC1857
    │   │   │   │   │   ├── [ 896]  CMSIS_Driver
    │   │   │   │   │   │   └── [  96]  Config
    │   │   │   │   │   ├── [ 128]  Device
    │   │   │   │   │   │   ├── [ 128]  Include
    │   │   │   │   │   │   └── [ 192]  Source
    │   │   │   │   │   │       ├── [  96]  ARM
    │   │   │   │   │   │       ├── [  96]  GCC
    │   │   │   │   │   │       └── [  96]  IAR
    │   │   │   │   │   ├── [ 256]  Documents
    │   │   │   │   │   ├── [ 320]  Flash
    │   │   │   │   │   │   └── [ 256]  LPC18xx43xx_IAP
    │   │   │   │   │   ├── [ 128]  Images
    │   │   │   │   │   └── [  96]  SVD
    │   │   │   │   └── [ 160]  Tutorials
    │   │   │   ├── [ 128]  RTOS
    │   │   │   │   ├── [ 288]  RTX
    │   │   │   │   │   ├── [ 128]  INC
    │   │   │   │   │   ├── [ 160]  LIB
    │   │   │   │   │   │   ├── [ 320]  ARM
    │   │   │   │   │   │   ├── [ 320]  GCC
    │   │   │   │   │   │   └── [ 256]  IAR
    │   │   │   │   │   ├── [1.1K]  SRC
    │   │   │   │   │   │   ├── [ 288]  ARM
    │   │   │   │   │   │   │   └── [  96]  RTE
    │   │   │   │   │   │   ├── [ 288]  GCC
    │   │   │   │   │   │   │   └── [  96]  RTE
    │   │   │   │   │   │   └── [ 256]  IAR
    │   │   │   │   │   ├── [  96]  Templates
    │   │   │   │   │   └── [ 352]  UserCodeTemplates
    │   │   │   │   └── [  96]  Template
    │   │   │   ├── [ 128]  SVD
    │   │   │   └── [ 192]  Utilities
    │   │   └── [ 160]  Device
    │   │       ├── [ 384]  ARM
    │   │       │   ├── [ 128]  ARMCM0
    │   │       │   │   ├── [ 128]  Include
    │   │       │   │   └── [ 192]  Source
    │   │       │   │       ├── [  96]  ARM
    │   │       │   │       ├── [ 160]  GCC
    │   │       │   │       └── [  96]  IAR
    │   │       │   ├── [ 128]  ARMCM0plus
    │   │       │   │   ├── [ 128]  Include
    │   │       │   │   └── [ 192]  Source
    │   │       │   │       ├── [  96]  ARM
    │   │       │   │       ├── [ 160]  GCC
    │   │       │   │       └── [  96]  IAR
    │   │       │   ├── [ 128]  ARMCM3
    │   │       │   │   ├── [ 128]  Include
    │   │       │   │   └── [ 192]  Source
    │   │       │   │       ├── [  96]  ARM
    │   │       │   │       ├── [ 160]  GCC
    │   │       │   │       └── [  96]  IAR
    │   │       │   ├── [ 128]  ARMCM4
    │   │       │   │   ├── [ 160]  Include
    │   │       │   │   └── [ 192]  Source
    │   │       │   │       ├── [  96]  ARM
    │   │       │   │       ├── [ 160]  GCC
    │   │       │   │       └── [  96]  IAR
    │   │       │   ├── [ 128]  ARMCM7
    │   │       │   │   ├── [ 192]  Include
    │   │       │   │   └── [ 192]  Source
    │   │       │   │       ├── [  96]  ARM
    │   │       │   │       ├── [ 160]  GCC
    │   │       │   │       └── [  96]  IAR
    │   │       │   ├── [ 128]  ARMSC000
    │   │       │   │   ├── [ 128]  Include
    │   │       │   │   └── [ 192]  Source
    │   │       │   │       ├── [  96]  ARM
    │   │       │   │       ├── [ 160]  GCC
    │   │       │   │       └── [  96]  IAR
    │   │       │   ├── [ 128]  ARMSC300
    │   │       │   │   ├── [ 128]  Include
    │   │       │   │   └── [ 192]  Source
    │   │       │   │       ├── [  96]  ARM
    │   │       │   │       ├── [ 160]  GCC
    │   │       │   │       └── [  96]  IAR
    │   │       │   ├── [ 224]  Documents
    │   │       │   ├── [  96]  Flash
    │   │       │   └── [ 320]  SVD
    │   │       ├── [ 320]  _Template_Flash
    │   │       │   └── [ 384]  Test
    │   │       └── [ 128]  _Template_Vendor
    │   │           └── [  96]  Vendor
    │   │               └── [ 128]  Device
    │   │                   ├── [ 128]  Include
    │   │                   └── [ 192]  Source
    │   │                       ├── [  96]  ARM
    │   │                       ├── [  96]  GCC
    │   │                       └── [  96]  IAR
    │   ├── [ 320]  avr
    │   │   ├── [ 384]  packs
    │   │   │   ├── [4.0K]  atmega
    │   │   │   ├── [ 992]  automotive
    │   │   │   ├── [1.9K]  tiny
    │   │   │   ├── [ 736]  xmegaa
    │   │   │   ├── [ 224]  xmegab
    │   │   │   ├── [ 352]  xmegac
    │   │   │   ├── [ 416]  xmegad
    │   │   │   └── [ 192]  xmegae
    │   │   └── [ 256]  src
    │   ├── [ 288]  cmsis-svd
    │   │   ├── [ 480]  data
    │   │   │   ├── [  96]  ARM_SAMPLE
    │   │   │   ├── [2.5K]  Atmel
    │   │   │   ├── [4.2K]  Freescale
    │   │   │   ├── [3.2K]  Fujitsu
    │   │   │   ├── [ 160]  Holtek
    │   │   │   ├── [ 800]  NXP
    │   │   │   ├── [ 128]  Nordic
    │   │   │   ├── [ 128]  Nuvoton
    │   │   │   ├── [1.8K]  STMicro
    │   │   │   ├── [ 384]  SiliconLabs
    │   │   │   ├── [2.8K]  Spansion
    │   │   │   ├── [1.6K]  TexasInstruments
    │   │   │   └── [ 256]  Toshiba
    │   │   └── [ 256]  python
    │   │       └── [ 288]  cmsis_svd
    │   │           ├── [ 160]  examples
    │   │           └── [ 160]  tests
    │   ├── [ 576]  compiler-rt
    │   │   ├── [ 224]  cmake
    │   │   │   ├── [ 320]  Modules
    │   │   │   └── [  96]  caches
    │   │   ├── [  96]  docs
    │   │   ├── [ 160]  include
    │   │   │   ├── [ 512]  sanitizer
    │   │   │   └── [ 160]  xray
    │   │   ├── [ 736]  lib
    │   │   │   ├── [ 192]  BlocksRuntime
    │   │   │   ├── [2.2K]  asan
    │   │   │   │   ├── [ 160]  scripts
    │   │   │   │   └── [ 736]  tests
    │   │   │   ├── [6.1K]  builtins
    │   │   │   │   ├── [ 512]  Darwin-excludes
    │   │   │   │   ├── [  96]  aarch64
    │   │   │   │   ├── [3.0K]  arm
    │   │   │   │   ├── [1.0K]  hexagon
    │   │   │   │   ├── [ 576]  i386
    │   │   │   │   ├── [ 256]  macho_embedded
    │   │   │   │   ├── [ 480]  ppc
    │   │   │   │   ├── [  96]  riscv
    │   │   │   │   └── [ 320]  x86_64
    │   │   │   ├── [ 160]  cfi
    │   │   │   ├── [ 448]  dfsan
    │   │   │   │   └── [ 128]  scripts
    │   │   │   ├── [ 736]  esan
    │   │   │   ├── [1.8K]  fuzzer
    │   │   │   │   ├── [  96]  afl
    │   │   │   │   ├── [  96]  dataflow
    │   │   │   │   ├── [ 160]  scripts
    │   │   │   │   ├── [  96]  standalone
    │   │   │   │   └── [ 128]  tests
    │   │   │   ├── [ 800]  hwasan
    │   │   │   ├── [ 416]  interception
    │   │   │   │   └── [ 192]  tests
    │   │   │   ├── [ 672]  lsan
    │   │   │   ├── [ 832]  msan
    │   │   │   │   └── [ 224]  tests
    │   │   │   ├── [ 800]  profile
    │   │   │   ├── [ 160]  safestack
    │   │   │   ├── [5.5K]  sanitizer_common
    │   │   │   │   ├── [ 256]  scripts
    │   │   │   │   ├── [ 160]  symbolizer
    │   │   │   │   │   └── [ 160]  scripts
    │   │   │   │   └── [1.2K]  tests
    │   │   │   ├── [ 832]  scudo
    │   │   │   ├── [ 192]  stats
    │   │   │   ├── [ 384]  tsan
    │   │   │   │   ├── [ 192]  benchmarks
    │   │   │   │   ├── [ 192]  dd
    │   │   │   │   ├── [ 192]  go
    │   │   │   │   ├── [2.1K]  rtl
    │   │   │   │   └── [ 160]  tests
    │   │   │   │       ├── [ 416]  rtl
    │   │   │   │       └── [ 416]  unit
    │   │   │   ├── [1.1K]  ubsan
    │   │   │   ├── [ 160]  ubsan_minimal
    │   │   │   └── [1.8K]  xray
    │   │   │       └── [ 128]  tests
    │   │   │           └── [ 320]  unit
    │   │   ├── [ 800]  test
    │   │   │   ├── [1.8K]  BlocksRuntime
    │   │   │   ├── [ 224]  asan
    │   │   │   │   ├── [5.7K]  TestCases
    │   │   │   │   │   ├── [  96]  Android
    │   │   │   │   │   ├── [1.2K]  Darwin
    │   │   │   │   │   ├── [ 448]  Helpers
    │   │   │   │   │   ├── [2.6K]  Linux
    │   │   │   │   │   ├── [1.8K]  Posix
    │   │   │   │   │   │   └── [ 160]  glob_test_root
    │   │   │   │   │   └── [3.2K]  Windows
    │   │   │   │   └── [  96]  Unit
    │   │   │   ├── [ 256]  builtins
    │   │   │   │   ├── [  96]  TestCases
    │   │   │   │   │   └── [ 160]  Darwin
    │   │   │   │   ├── [5.8K]  Unit
    │   │   │   │   │   ├── [ 416]  arm
    │   │   │   │   │   ├── [ 416]  ppc
    │   │   │   │   │   └── [  96]  riscv
    │   │   │   │   └── [ 640]  timing
    │   │   │   ├── [ 864]  cfi
    │   │   │   │   ├── [ 320]  cross-dso
    │   │   │   │   │   ├── [ 224]  icall
    │   │   │   │   │   └── [  96]  util
    │   │   │   │   └── [ 224]  icall
    │   │   │   ├── [ 512]  dfsan
    │   │   │   │   └── [  96]  Inputs
    │   │   │   ├── [ 224]  esan
    │   │   │   │   ├── [ 416]  TestCases
    │   │   │   │   └── [ 128]  Unit
    │   │   │   ├── [6.1K]  fuzzer
    │   │   │   │   └── [  96]  unit
    │   │   │   ├── [ 192]  hwasan
    │   │   │   │   └── [ 320]  TestCases
    │   │   │   │       ├── [ 160]  Linux
    │   │   │   │       └── [ 128]  Posix
    │   │   │   ├── [ 128]  interception
    │   │   │   │   └── [  96]  Unit
    │   │   │   ├── [ 192]  lsan
    │   │   │   │   └── [1.1K]  TestCases
    │   │   │   │       ├── [ 128]  Darwin
    │   │   │   │       ├── [ 448]  Linux
    │   │   │   │       └── [  96]  Posix
    │   │   │   ├── [3.8K]  msan
    │   │   │   │   ├── [ 928]  Linux
    │   │   │   │   │   ├── [ 160]  glob_test_root
    │   │   │   │   │   └── [  96]  xattr_test_root
    │   │   │   │   ├── [  96]  Unit
    │   │   │   │   └── [ 160]  scandir_test_root
    │   │   │   ├── [1.7K]  profile
    │   │   │   │   ├── [1.9K]  Inputs
    │   │   │   │   ├── [ 768]  Linux
    │   │   │   │   └── [  96]  Posix
    │   │   │   ├── [ 448]  safestack
    │   │   │   ├── [ 320]  sanitizer_common
    │   │   │   │   ├── [1.1K]  TestCases
    │   │   │   │   │   ├── [ 160]  Darwin
    │   │   │   │   │   ├── [1.1K]  Linux
    │   │   │   │   │   ├── [ 480]  NetBSD
    │   │   │   │   │   └── [ 896]  Posix
    │   │   │   │   ├── [  96]  Unit
    │   │   │   │   ├── [ 160]  android_commands
    │   │   │   │   └── [ 192]  ios_commands
    │   │   │   ├── [ 928]  scudo
    │   │   │   ├── [ 352]  shadowcallstack
    │   │   │   ├── [7.8K]  tsan
    │   │   │   │   ├── [2.4K]  Darwin
    │   │   │   │   ├── [ 352]  Linux
    │   │   │   │   ├── [  96]  Unit
    │   │   │   │   └── [ 128]  libcxx
    │   │   │   ├── [ 192]  ubsan
    │   │   │   │   └── [ 256]  TestCases
    │   │   │   │       ├── [  96]  Float
    │   │   │   │       ├── [ 160]  ImplicitConversion
    │   │   │   │       ├── [ 544]  Integer
    │   │   │   │       ├── [ 640]  Misc
    │   │   │   │       │   ├── [ 128]  Inputs
    │   │   │   │       │   └── [ 160]  Linux
    │   │   │   │       ├── [ 128]  Pointer
    │   │   │   │       └── [ 384]  TypeCheck
    │   │   │   │           ├── [ 128]  Function
    │   │   │   │           └── [ 128]  Linux
    │   │   │   ├── [ 192]  ubsan_minimal
    │   │   │   │   └── [ 224]  TestCases
    │   │   │   └── [ 224]  xray
    │   │   │       ├── [  96]  TestCases
    │   │   │       │   └── [ 896]  Posix
    │   │   │       └── [  96]  Unit
    │   │   ├── [ 192]  unittests
    │   │   ├── [ 128]  utils
    │   │   └── [ 192]  www
    │   └── [ 448]  nrfx
    │       ├── [ 448]  doc
    │       │   ├── [ 288]  buildfiles
    │       │   └── [ 960]  config_dox
    │       ├── [ 192]  drivers
    │       │   ├── [ 992]  include
    │       │   └── [ 992]  src
    │       │       └── [ 128]  prs
    │       ├── [1.2K]  hal
    │       ├── [2.2K]  mdk
    │       ├── [ 256]  soc
    │       └── [ 256]  templates
    │           ├── [  96]  nRF51
    │           ├── [  96]  nRF52810
    │           ├── [  96]  nRF52832
    │           └── [  96]  nRF52840
    ├── [ 256]  loader
    ├── [ 288]  src
    │   ├── [ 192]  device
    │   │   ├── [ 128]  arm
    │   │   ├── [  96]  avr
    │   │   ├── [  96]  nrf
    │   │   └── [  96]  stm32
    │   ├── [ 480]  examples
    │   │   ├── [  96]  adc
    │   │   ├── [  96]  blinkm
    │   │   ├── [  96]  blinky1
    │   │   ├── [  96]  blinky2
    │   │   ├── [  96]  button
    │   │   ├── [  96]  button2
    │   │   ├── [  96]  colorlamp
    │   │   ├── [  96]  echo
    │   │   ├── [  96]  mcp3008
    │   │   ├── [  96]  pwm
    │   │   ├── [  96]  serial
    │   │   ├── [  96]  test
    │   │   └── [ 160]  wasm
    │   ├── [ 800]  machine
    │   ├── [ 160]  reflect
    │   ├── [1.2K]  runtime
    │   │   ├── [  96]  cgo
    │   │   └── [  96]  internal
    │   │       └── [  96]  sys
    │   ├── [ 160]  sync
    │   └── [  96]  unsafe
    ├── [ 896]  targets
    ├── [ 864]  testdata
    │   └── [ 192]  cgo
    └── [ 128]  tools

505 directories, 0 files
```

