# SPDX-License-Identifier: GPL-2.0
#
# Makefile for the Linux network (wan) device drivers.
#
# 3 Aug 2000, Christoph Hellwig <hch@infradead.org>
# Rewritten to use lists instead of if-statements.
#

obj-$(CONFIG_HDLC)		+= hdlc.o
obj-$(CONFIG_HDLC_RAW)		+= hdlc_raw.o
obj-$(CONFIG_HDLC_RAW_ETH)	+= hdlc_raw_eth.o
obj-$(CONFIG_HDLC_CISCO)	+= hdlc_cisco.o
obj-$(CONFIG_HDLC_FR)		+= hdlc_fr.o
obj-$(CONFIG_HDLC_PPP)		+= hdlc_ppp.o
obj-$(CONFIG_HDLC_X25)		+= hdlc_x25.o

obj-$(CONFIG_HOSTESS_SV11)	+= z85230.o	hostess_sv11.o
obj-$(CONFIG_SEALEVEL_4021)	+= z85230.o	sealevel.o
obj-$(CONFIG_COSA)		+= cosa.o
obj-$(CONFIG_FARSYNC)		+= farsync.o

obj-$(CONFIG_LANMEDIA)		+= lmc/

obj-$(CONFIG_DLCI)		+= dlci.o 
obj-$(CONFIG_SDLA)		+= sdla.o
obj-$(CONFIG_LAPBETHER)		+= lapbether.o
obj-$(CONFIG_SBNI)		+= sbni.o
obj-$(CONFIG_N2)		+= n2.o
obj-$(CONFIG_C101)		+= c101.o
obj-$(CONFIG_WANXL)		+= wanxl.o
obj-$(CONFIG_PCI200SYN)		+= pci200syn.o
obj-$(CONFIG_PC300TOO)		+= pc300too.o
obj-$(CONFIG_IXP4XX_HSS)	+= ixp4xx_hss.o
obj-$(CONFIG_FSL_UCC_HDLC)	+= fsl_ucc_hdlc.o
obj-$(CONFIG_SLIC_DS26522)	+= slic_ds26522.o

clean-files := wanxlfw.inc
$(obj)/wanxl.o:	$(obj)/wanxlfw.inc

ifeq ($(CONFIG_WANXL_BUILD_FIRMWARE),y)
ifeq ($(ARCH),m68k)
  M68KCC = $(CC)
  M68KLD = $(LD)
else
  M68KCC = $(CROSS_COMPILE_M68K)gcc
  M68KLD = $(CROSS_COMPILE_M68K)ld
endif

quiet_cmd_build_wanxlfw = BLDFW   $@
      cmd_build_wanxlfw = hexdump -ve '"\n" 16/1 "0x%02X,"' $< | \
	sed 's/0x  ,//g;1s/^/static const u8 firmware[]={/;$$s/,$$/\n};\n/' > $@

$(obj)/wanxlfw.inc: $(obj)/wanxlfw.bin FORCE
	$(call if_changed,build_wanxlfw)

quiet_cmd_m68kld_bin_o = M68KLD  $@
      cmd_m68kld_bin_o = $(M68KLD) --oformat binary -Ttext 0x1000 $< -o $@

$(obj)/wanxlfw.bin: $(obj)/wanxlfw.o FORCE
	$(call if_changed,m68kld_bin_o)

quiet_cmd_m68kas_o_S = M68KAS  $@
      cmd_m68kas_o_S = $(M68KCC) -D__ASSEMBLY__ -Wp,-MD,$(depfile) -I$(srctree)/include/uapi -c -o $@ $<

$(obj)/wanxlfw.o: $(src)/wanxlfw.S FORCE
	$(call if_changed_dep,m68kas_o_S)
endif
targets += wanxlfw.inc wanxlfw.bin wanxlfw.o
