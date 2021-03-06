SHELL = /bin/sh

PREFIX=/usr/local
INSTDIR=$(DESTDIR)/$(PREFIX)/bin
MANDIR=$(DESTDIR)/$(PREFIX)/man

CC = gcc
LD = gcc

EXECS = sg_iovec_tst sg_sense_test sg_queue_tst bsg_queue_tst sg_chk_asc \
	tst_sg_lib
	
EXTRAS =

BSG_EXTRAS =


MAN_PGS = 
MAN_PREF = man8

LARGE_FILE_FLAGS = -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64

# For C++/clang testing
# ## CC = gcc
# ## CC = g++
# ## CC = clang
# ## CC = clang++

CFLAGS = -g -O2 -W -Wall -iquote ../include -D_REENTRANT $(LARGE_FILE_FLAGS)
# CFLAGS = -g -O2 -Wall -iquote ../include -D_REENTRANT -DSG_KERNEL_INCLUDES $(LARGE_FILE_FLAGS)
# CFLAGS = -g -O2 -Wall -pedantic -iquote ../include -D_REENTRANT $(LARGE_FILE_FLAGS)
# CFLAGS = -Wall -W -iquote ../include -D_REENTRANT $(LARGE_FILE_FLAGS) -pedantic -std=c11 --analyze
# CFLAGS = -Wall -W -iquote ../include -D_REENTRANT $(LARGE_FILE_FLAGS) -pedantic -std=c++14

LDFLAGS =

LIBFILESOLD = ../lib/sg_lib.o ../lib/sg_lib_data.o ../lib/sg_io_linux.o
LIBFILESNEW = ../lib/sg_lib.o ../lib/sg_lib_data.o ../lib/sg_pt_linux.o

all: $(EXECS)

extras: $(EXTRAS)

bsg: $(BSG_EXTRAS)


depend dep:
	for i in *.c; do $(CC) $(INCLUDES) $(CFLAGS) -M $$i; \
	done > .depend

clean:
	/bin/rm -f *.o $(EXECS) $(EXTRAS) $(BSG_EXTRAS) core .depend

sg_iovec_tst: sg_iovec_tst.o $(LIBFILESOLD)
	$(LD) -o $@ $(LDFLAGS) $^

sg_sense_test: sg_sense_test.o $(LIBFILESOLD)
	$(LD) -o $@ $(LDFLAGS) $^

sg_queue_tst: sg_queue_tst.o $(LIBFILESOLD)
	$(LD) -o $@ $(LDFLAGS) $^ 

bsg_queue_tst: bsg_queue_tst.o $(LIBFILESOLD)
	$(LD) -o $@ $(LDFLAGS) $^ 

# building sg_chk_asc depends on a prior successful make in ../lib
sg_chk_asc: sg_chk_asc.o ../lib/sg_lib.o ../lib/sg_lib_data.o
	$(LD) -o $@ $(LDFLAGS) $^

tst_sg_lib: tst_sg_lib.o ../lib/sg_lib.o ../lib/sg_lib_data.o
	$(LD) -o $@ $(LDFLAGS) $^

install: $(EXECS)
	install -d $(INSTDIR)
	for name in $^; \
	 do install -s -o root -g root -m 755 $$name $(INSTDIR); \
	done
	install -d $(MANDIR)/$(MAN_PREF)
	for mp in $(MAN_PGS); \
	 do install -o root -g root -m 644 $$mp $(MANDIR)/$(MAN_PREF); \
	 gzip -9f $(MANDIR)/$(MAN_PREF)/$$mp; \
	done

uninstall:
	dists="$(EXECS)"; \
	for name in $$dists; do \
	 rm -f $(INSTDIR)/$$name; \
	done
	for mp in $(MAN_PGS); do \
	 rm -f $(MANDIR)/$(MAN_PREF)/$$mp.gz; \
	done

ifeq (.depend,$(wildcard .depend))
include .depend
endif
