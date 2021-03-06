-include ../../../config-host.mak

CROSS=xtensa-dc232b-elf-

ifndef XT
SIM = ../../../xtensa-softmmu/qemu-system-xtensa
SIMFLAGS = -M sim -cpu dc232b -nographic -semihosting $(EXTFLAGS) -kernel
SIMDEBUG = -s -S
else
SIM = xt-run
SIMFLAGS = --xtensa-core=DC_B_232L --exit_with_target_code $(EXTFLAGS)
SIMDEBUG = --gdbserve=0
endif

CC      = $(CROSS)gcc
AS      = $(CROSS)gcc -x assembler-with-cpp
LD      = $(CROSS)ld

XTENSA_SRC_PATH = $(SRC_PATH)/tests/tcg/xtensa

LDFLAGS = -T$(XTENSA_SRC_PATH)/linker.ld

CRT        = crt.o vectors.o

TESTCASES += test_b.tst
TESTCASES += test_bi.tst
#TESTCASES += test_boolean.tst
TESTCASES += test_break.tst
TESTCASES += test_bz.tst
TESTCASES += test_clamps.tst
TESTCASES += test_extui.tst
TESTCASES += test_fail.tst
TESTCASES += test_interrupt.tst
TESTCASES += test_loop.tst
TESTCASES += test_mac16.tst
TESTCASES += test_max.tst
TESTCASES += test_min.tst
TESTCASES += test_mmu.tst
TESTCASES += test_mul16.tst
TESTCASES += test_mul32.tst
TESTCASES += test_nsa.tst
ifdef XT
TESTCASES += test_pipeline.tst
endif
TESTCASES += test_quo.tst
TESTCASES += test_rem.tst
TESTCASES += test_rst0.tst
TESTCASES += test_s32c1i.tst
TESTCASES += test_sar.tst
TESTCASES += test_sext.tst
TESTCASES += test_shift.tst
TESTCASES += test_sr.tst
TESTCASES += test_timer.tst
TESTCASES += test_windowed.tst

all: build

%.o: $(XTENSA_SRC_PATH)/%.c
	$(CC) -I$(XTENSA_SRC_PATH) $(CFLAGS) -c $< -o $@

%.o: $(XTENSA_SRC_PATH)/%.S
	$(AS) -Wa,-I,$(XTENSA_SRC_PATH) $(ASFLAGS) -c $< -o $@

%.tst: %.o $(XTENSA_SRC_PATH)/macros.inc $(CRT) Makefile
	$(LD) $(LDFLAGS) $(NOSTDFLAGS) $(CRT) $< -o $@

build: $(TESTCASES)

check: $(addprefix run-, $(TESTCASES))

run-%.tst: %.tst
	$(SIM) $(SIMFLAGS) ./$<

run-test_fail.tst: test_fail.tst
	! $(SIM) $(SIMFLAGS) ./$<

debug-%.tst: %.tst
	$(SIM) $(SIMDEBUG) $(SIMFLAGS) ./$<

host-debug-%.tst: %.tst
	gdb --args $(SIM) $(SIMFLAGS) ./$<

clean:
	$(RM) -fr $(TESTCASES) $(CRT)
