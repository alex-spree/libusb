AUTOMAKE_OPTIONS = subdir-objects

AM_CFLAGS += -fvisibility=hidden $(THREAD_CFLAGS)
AM_CXXFLAGS += -fvisibility=hidden $(THREAD_CFLAGS)
AM_LDFLAGS = $(LT_LDFLAGS)

lib_LTLIBRARIES = libusb-1.0.la

POSIX_POLL_SRC = os/poll_posix.h os/poll_posix.c
WINDOWS_POLL_SRC = os/poll_windows.h os/poll_windows.c

if POLL_POSIX
POLL_SRC = $(POSIX_POLL_SRC)
else
POLL_SRC = $(WINDOWS_POLL_SRC)
endif

POSIX_THREADS_SRC = os/threads_posix.h os/threads_posix.c
WINDOWS_THREADS_SRC = os/threads_windows.h os/threads_windows.c

if THREADS_POSIX
THREADS_SRC = $(POSIX_THREADS_SRC)
else
THREADS_SRC = $(WINDOWS_THREADS_SRC)
endif

OS_DARWIN_SRC = os/darwin_usb.h os/darwin_usb.c
OS_HAIKU_SRC = os/haiku_usb.h os/haiku_usb_backend.cpp \
	       os/haiku_pollfs.cpp os/haiku_usb_raw.h os/haiku_usb_raw.cpp
OS_LINUX_SRC = os/linux_usbfs.h os/linux_usbfs.c
OS_NETBSD_SRC = os/netbsd_usb.c
OS_NULL_SRC = os/null_usb.c
OS_OPENBSD_SRC = os/openbsd_usb.c
OS_SUNOS_SRC = os/sunos_usb.h os/sunos_usb.c
OS_WINDOWS_SRC = libusb-1.0.def libusb-1.0.rc \
		 os/windows_common.h os/windows_common.c \
		 os/windows_usbdk.h os/windows_usbdk.c \
		 os/windows_winusb.h os/windows_winusb.c

if OS_DARWIN
OS_SRC = $(OS_DARWIN_SRC)
endif

if OS_HAIKU
noinst_LTLIBRARIES = libusb_haiku.la
libusb_haiku_la_SOURCES = $(OS_HAIKU_SRC)
libusb_1_0_la_LIBADD = libusb_haiku.la
endif

if OS_LINUX
OS_SRC = $(OS_LINUX_SRC)
if USE_UDEV
OS_SRC += os/linux_udev.c
else
OS_SRC += os/linux_netlink.c
endif
endif

if OS_NETBSD
OS_SRC = $(OS_NETBSD_SRC)
endif

if OS_NULL
OS_SRC = $(OS_NULL_SRC)
endif

if OS_OPENBSD
OS_SRC = $(OS_OPENBSD_SRC)
endif

if OS_SUNOS
OS_SRC = $(OS_SUNOS_SRC)
endif

if OS_WINDOWS
OS_SRC = $(OS_WINDOWS_SRC)

include Makefile.am.extra

# Dependencies for compiling libusb-1.0.lo from libusb-1.0.rc
-include ./$(DEPDIR)/libusb-1.0.Plo

if CREATE_IMPORT_LIB
all-local: .libs/libusb-1.0.dll.a
# Rebuild the import lib from the .def so that MS and MinGW DLLs can be interchanged
.libs/libusb-1.0.dll.a: libusb-1.0.def libusb-1.0.la
	$(AM_V_DLLTOOL)$(DLLTOOL) $(DLLTOOLFLAGS) --kill-at --input-def $< --dllname libusb-1.0.dll --output-lib $@
endif
endif

libusb_1_0_la_SOURCES = libusbi.h version.h version_nano.h \
	core.c descriptor.c hotplug.h hotplug.c io.c strerror.c sync.c \
	$(POLL_SRC) $(THREADS_SRC) $(OS_SRC)

pkginclude_HEADERS = libusb.h
