
CFLAGS += -I../include -I. -g -Wall -Werror -pie
EXEEXT =

SYSTEMD ?= 1

ifneq ($(OS),Windows_NT)
CFLAGS += -fPIC -O2
ifeq ($(SYSTEMD),1)
CFLAGS += `pkg-config --cflags libsystemd-daemon` -DHAVE_SYSTEMD
LIBS += `pkg-config --libs libsystemd-daemon`
endif
CFLAGS += `pkg-config --cflags vchan-$(BACKEND_VMM)`
LIBS += `pkg-config --libs vchan-$(BACKEND_VMM)`

else # Windows_NT
EXEEXT = .exe
CFLAGS += -I$(QUBES_INCLUDES)
CFLAGS += -DUNICODE -D_UNICODE
LIBS += -L$(QUBES_LIBS) -lvchan-$(BACKEND_VMM)
endif
CFLAGS += -DBACKEND_VMM_$(BACKEND_VMM)

SBINDIR = /usr/sbin

all: qubesdb-daemon$(EXEEXT)

install:
	install -d $(DESTDIR)$(SBINDIR)
	install qubesdb-daemon$(EXEEXT) $(DESTDIR)$(SBINDIR)/

qubesdb-daemon$(EXEEXT): db-cmds.o db-daemon.o db-core.o buffer.o
	$(CC) $(LDFLAGS) -o $@ $^ $(APPEND_LDFLAGS) $(LIBS)

clean:
	rm -f *.o qubesdb-daemon$(EXEEXT)
