
CSRCS = devtty.c
CSRCS += devices.c main.c libc.c

ASRCS = p68000.S crt0.S
ASRCS += tricks.S

DSRCS = ../dev/devide.c ../dev/mbr.c ../dev/blkdev.c ../dev/devide_discard.c
DOBJS = $(patsubst ../dev/%.c,%.o, $(DSRCS))

COBJS = $(CSRCS:.c=$(BINEXT))
AOBJS = $(ASRCS:.S=.o)
OBJS  = $(COBJS) $(AOBJS) $(DOBJS)

CROSS_CCOPTS += -I../dev/

JUNK = $(CSRCS:.c=.o) $(ASRCS:.S=.o)

all:	$(OBJS)

$(COBJS): %.o: %.c
	$(CROSS_CC) $(CROSS_CCOPTS) -c $<

$(AOBJS): %.o: %.S
	$(CROSS_AS) $(ASOPTS) $< -o $*.o

$(DOBJS): %.o: ../dev/%.c
	$(CROSS_CC) $(CROSS_CCOPTS) -c $<

clean:
	rm -f $(OBJS) $(JUNK)  core *~ 

image:
	$(CROSS_LD) -M -o fuzix.elf -T fuzix.ld \
	crt0.o \
	p68000.o ../start.o ../version.o ../lowlevel-68000.o \
	tricks.o ../bankfixed.o main.o ../timer.o ../kdata.o devices.o \
	../devio.o ../filesys.o ../process.o ../inode.o ../syscall_fs.o \
	../syscall_proc.o ../syscall_other.o ../mm.o ../flat_mem.o \
	../tty.o ../devsys.o ../usermem.o ../syscall_fs2.o \
	../syscall_fs3.o ../syscall_exec32.o blkdev.o devide.o devide_discard.o mbr.o  \
	../usermem_std-68000.o devtty.o libc.o ../malloc.o > ../fuzix.map
	m68k-linux-gnu-objcopy fuzix.elf -O binary ../fuzix.bin
