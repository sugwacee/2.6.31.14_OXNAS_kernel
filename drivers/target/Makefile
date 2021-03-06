##################################################################################
# Filename:  Makefile
#
# Copyright (c) 2002, 2003, 2004, 2005 PyX Technologies, Inc.
# Copyright (c) 2005, 2006, 2007 SBE, Inc. 
# Copyright (c) 2007 Rising Tide Software, Inc.
#
# Nicholas A. Bellinger <nab@kernel.org>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA 02111-1307, USA.
#
##################################################################################

CWD=$(shell pwd)
#AUTOCONFIG=$(CWD)/../../../autoconfig --write-to-file --current-directory=$(CWD)
#include $(shell $(AUTOCONFIG))
include $(CWD)/drivers/target/MCONFIG_TARGET
include $(CWD)/drivers/target/MCONFIG_ALL

obj-$(CONFIG_LIO_TARGET)	+=	target_core_mod.o
target_core_mod-objs			:=	target_core_configfs.o \
						target_core_device.o \
						target_core_hba.o \
						target_core_plugin.o \
						target_core_pr.o \
						target_core_alua.o \
						target_core_scdb.o \
						target_core_seobj.o \
						target_core_tmr.o \
						target_core_tpg.o \
						target_core_transport.o \
						target_core_ua.o \
						div64.o

ifeq ($(LINUX_IBLOCK), 1)
target_core_mod-objs			+=	target_core_iblock.o
EXTRA_CFLAGS				+=	-DPYX_IBLOCK
endif
ifeq ($(LINUX_PARALLEL_SCSI), 1)
target_core_mod-objs			+=	target_core_pscsi.o
EXTRA_CFLAGS				+=	-DPARALLEL_SCSI
endif
ifeq ($(LINUX_STGT), 1)
target_core_mod-objs			+=	target_core_stgt.o
EXTRA_CFLAGS				+=	-DSTGT_PLUGIN
endif
ifeq ($(LINUX_RAMDISK), 1)
target_core_mod-objs			+=	target_core_rd.o
EXTRA_CFLAGS				+=	-DPYX_RAMDISK
endif
ifeq ($(LINUX_FILEIO), 1)
target_core_mod-objs			+=	target_core_file.o
EXTRA_CFLAGS				+=	-DPYX_FILEIO
endif
ifeq ($(SNMP_FEATURE), 1)
target_core_mod-objs			+=	target_core_mib.o
EXTRA_CFLAGS				+=	-DSNMP_SUPPORT
endif
ifeq ($(LINUX_VPD_PAGE_CHECK), 1)
EXTRA_CFLAGS += -DLINUX_VPD_PAGE_CHECK
endif
ifeq ($(LINUX_SCSI_MEDIA_ROM), 1)
EXTRA_CFLAGS += -DLINUX_SCSI_MEDIA_ROM
endif
ifeq ($(DEBUG_DEV), 1)
EXTRA_CFLAGS += -DDEBUG_DEV
endif


EXTRA_CFLAGS+=-I$(CWD)/drivers/target/ -I$(CWD)/drivers/scsi/
# -I$(CWD)/../../include/ -I$(CWD)
EXTRA_CFLAGS+=-D_TARGET -DLINUX -DLINUX_KERNEL_26 -DLINUX_SCSI_HOST_LOCK -DLINUX_USE_SIGHAND
EXTRA_CFLAGS+=-DLINUX_SCATTERLIST_HAS_PAGE -DPYX_ISCSI_VENDOR='"Linux-iSCSI.org"'

all:
	$(MAKE) -C $(KERNEL_DIR) SUBDIRS=$(CWD) modules CWD=$(CWD) ARCH=$(ARCH) KBUILD_VERBOSE=0

#install ins:
#	mkdir -p /lib/modules/`uname -r`/extra
#	cp -f target_core_mod.ko /lib/modules/`uname -r`/extra
#	depmod -ae
#
#
#
#clean:
#	rm -f $(foreach prog,$(target_core_mod-objs) $(obj-m),$(CWD)/$(prog)) $(CWD)/target_core_mod.mod.o
#	rm -f target_core_mod.ko target_core_mod.mod.c
#	rm -f .*.cmd ../common/.*.cmd .make_autoconfig *~
#	rm -fr .tmp_versions


