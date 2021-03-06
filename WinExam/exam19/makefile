#  Makefile designed for use by the GNUMAKE utility and		-*-Makefile-*-
#  the Wine libraries

CC = winegcc -m32
GCC = gcc -m32

.SUFFIXES: .d


DYNACE_PATH = ../..

WINEINCPATH = /usr/include/wine


BINDIR = $(DYNACE_PATH)/bin
LIBDIR = $(DYNACE_PATH)/lib
INCDIR = $(DYNACE_PATH)/include

WINEINC = -I$(WINEINCPATH)/windows -I$(WINEINCPATH)/msvcrt


WINELIBS = -lkernel32 -luser32 -lgdi32 -lcomdlg32 -lcomctl32 -ladvapi32 -lshell32 -lmpr -lwine -lmsvcrt -lmsimg32

DEBUG = -g
#DEBUG = -O

CFLAGS = -I$(INCDIR) $(WINEINC) -D_WINDOWS
GFLAGS = -I$(INCDIR) $(WINEINC) -D_WINDOWS -D_WIN32 -O -w

TARGET = main

CLASSES = class1.d

CFILES = main.c

OBJS = $(CFILES:.c=.o) $(CLASSES:.d=.o)

%.res : %.rc
	echo '#include <winuser.h>' >temp.rc
	echo '#include <commctrl.h>' >>temp.rc
	cat $< >>temp.rc
	wrc temp.rc
	rm -f temp.rc
	mv temp.res $@

%.c : %.d
	$(BINDIR)/dpp -g -p $<

%.o : %.c
	$(CC) $(CFLAGS) $(DEBUG) -c $<


$(TARGET) : generics.1 algocorp.ico generics.h generics.o $(OBJS) main.res
	$(CC) $(DEBUG) -o $(TARGET) $(OBJS) generics.o main.res $(LIBDIR)/wds.a $(LIBDIR)/dynace.a $(WINELIBS) -lm 

algocorp.ico : ../setup/algocorp.ico
	cp $^ $@

generics.h : $(CLASSES)
	$(BINDIR)/dpp -g -t -h -p $?

generics.c : generics.h
	$(BINDIR)/dpp -g -c

# generics.c must be compiled with gcc and not winegcc!
generics.o : generics.c 
	$(GCC) $(GFLAGS) -c $<

generics.1 newgens:
	$(BINDIR)/dpp -g $(INCDIR)/generics.h -p $(CLASSES) -h
	touch generics.1

clean:
	-rm -f *.obj *.exe generics.* *.o *~ main.res temp.rc $(CLASSES:.d=.c)

realclean: clean
	-rm -f $(TARGET) $(TARGET).exe.so
