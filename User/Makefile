## @file
# Copyright (c) 2020, PMheart. All rights reserved.
# SPDX-License-Identifier: BSD-3-Clause
##

CC       ?= gcc

UDK_PATH ?= ../../UDK
UDK_ARCH ?= X64

ifeq ($(OS),Windows_NT)
	DIST ?= Windows
else
	DIST ?= $(shell uname)
endif

CFLAGS    = -c -fshort-wchar -Wall -Wextra -Wno-implicit-fallthrough -Wno-unused-parameter
CFLAGS   += -I../../User/Include
CFLAGS   += -I../../Include/Acidanthera -I../../Include/Apple -I../../Include/Generic -I../../Include/Intel

ifeq ($(DIST),Darwin)
	CFLAGS  += -mmacosx-version-min=10.6
	LDFLAGS += -mmacosx-version-min=10.6
endif

ifeq ($(DIST),Windows)
	SUFFIX  := .exe
endif

ifeq ($(SANITIZE),1)
	CFLAGS  += -fsanitize=undefined,address
	LDFLAGS += -fsanitize=undefined,address
endif

ifeq ($(DEBUG),1)
	CFLAGS  += -g -O0
	STRIP   := @echo No strip in DEBUG mode
else
	CFLAGS  += -O3
	STRIP   := strip -x
endif

#
# Skip including UDK when suggesting STANDALONE mode.
#
ifneq ($(STANDALONE),1)
	CFLAGS  += -D NO_MSABI_VA_FUNCS
	CFLAGS  += -I$(UDK_PATH)/MdePkg/Include -I$(UDK_PATH)/MdePkg/Include/Library -I$(UDK_PATH)/MdePkg/Include/$(UDK_ARCH)
	CFLAGS  += -I$(UDK_PATH)/UefiCpuPkg/Include
	#
	# Compatibility headers.
	#
	CFLAGS  += -include ../../User/Include/Pcd.h -include ../../User/Include/EfiVar.h
	#
	# UDK implementations.
	#
	OBJS     = UefiLibPrint.o CpuDeadLoop.o BaseDebugPrintErrorLevelLib.o DebugLib.o PrintLib.o PrintLibInternal.o String.o SafeString.o SwapBytes16.o SwapBytes32.o LinkedList.o
	#
	# Customised/Simplified implementations at userspace level.
	#
	OBJS    += BaseMemoryLib.o BootServices.o DebugBreak.o EfiVar.o Math.o Pcd.o
	#
	# OcGuardLib targets.
	#
	OBJS    += BitOverflow.o NativeOverflow.o TripleOverflow.o
endif

UefiLibPrint.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/UefiLib/UefiLibPrint.c -o $@

CpuDeadLoop.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BaseLib/CpuDeadLoop.c -o $@

BaseDebugPrintErrorLevelLib.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BaseDebugPrintErrorLevelLib/BaseDebugPrintErrorLevelLib.c -o $@

DebugLib.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/UefiDebugLibConOut/DebugLib.c -o $@

PrintLib.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BasePrintLib/PrintLib.c -o $@

PrintLibInternal.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BasePrintLib/PrintLibInternal.c -o $@

String.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BaseLib/String.c -o $@

SafeString.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BaseLib/SafeString.c -o $@

SwapBytes16.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BaseLib/SwapBytes16.c -o $@

SwapBytes32.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BaseLib/SwapBytes32.c -o $@

LinkedList.o:
	$(CC) $(CFLAGS) $(UDK_PATH)/MdePkg/Library/BaseLib/LinkedList.c -o $@

BaseMemoryLib.o:
	$(CC) $(CFLAGS) ../../User/Library/BaseMemoryLib.c -o $@

BootServices.o:
	$(CC) $(CFLAGS) ../../User/Library/BootServices.c -o $@

DebugBreak.o:
	$(CC) $(CFLAGS) ../../User/Library/DebugBreak.c -o $@

EfiVar.o:
	$(CC) $(CFLAGS) ../../User/Library/EfiVar.c -o $@

Math.o:
	$(CC) $(CFLAGS) ../../User/Library/Math.c -o $@

Pcd.o:
	$(CC) $(CFLAGS) ../../User/Library/Pcd.c -o $@

BitOverflow.o:
	$(CC) $(CFLAGS) ../../Library/OcGuardLib/BitOverflow.c -o $@

NativeOverflow.o:
	$(CC) $(CFLAGS) ../../Library/OcGuardLib/NativeOverflow.c -o $@

TripleOverflow.o:
	$(CC) $(CFLAGS) ../../Library/OcGuardLib/TripleOverflow.c -o $@
