# $KmKId: makefile,v 1.47 2023-04-01 14:20:47+00 kentd Exp $

XOPTS_WIN = -Wall -fomit-frame-pointer -march=pentium

SWIFTOBJS = AppDelegate.o MainView.o

include vars
include ldvars

.SUFFIXES: .dep .proto

AS = $(CC)

XLIBS = -L/usr/X11/lib
PERL = perl

all: $(TARGET)

specials:

specials_clean:

clean:
	rm -f *.o kegsmac


# Mac builds:
kegsmac: $(OBJECTS) $(OBJECTS1) compile_time.o $(SWIFTOBJS)
	clang $(CCOPTS) $(LDOPTS) $(OBJECTS) $(OBJECTS1) $(SWIFTOBJS) \
		compile_time.o $(LDFLAGS) -o kegsmac $(EXTRA_LIBS) \
		-isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk \
		-Xlinker -rpath -Xlinker @executable_path/../Frameworks \
		-Xlinker -rpath -Xlinker /usr/lib/swift \
		-Xlinker -no_deduplicate -fobjc-link-runtime \
		-L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/macosx \
		-L/usr/lib/swift
	mkdir -p ../Kegs.app/Contents/Resources/
	mkdir -p ../Kegs.app/Contents/MacOS
	mkdir -p ../Kegs.app/Contents/Frameworks
	$(PERL) cp_kegs_libs kegsmac /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/macosx ../Kegs.app/Contents/Frameworks
	mv kegsmac ../Kegs.app/Contents/MacOS/kegs
	echo "APPL????" > ../Kegs.app/Contents/PkgInfo
	cp -f Info.plist ../Kegs.app/Contents/
	cp -f $(PROJROOT)/lib/MainMenu.nib ../Kegs.app/Contents/Resources/
	cp -f $(PROJROOT)/lib/kegsiconalt.icns ../Kegs.app/Contents/Resources/kegsicon.icns
	## UNCOMMENT this to use the original kegs icon
	## cp -f $(PROJROOT)/lib/kegsicon.icns ../Kegs.app/Contents/Resources/
	
	#cp -f $(PROJROOT)/lib/2mg.icns ../Kegs.app/Contents/Resources/
	#cp -f $(PROJROOT)/lib/525.icns ../Kegs.app/Contents/Resources/
	#touch '../Kegs.app/Icon?'

# Linux for X builds:
xkegs: $(OBJECTS) $(OBJECTS1) compile_time.o
	$(CC) $(CCOPTS) $(LDOPTS) $(OBJECTS) $(OBJECTS1) compile_time.o \
		$(LDFLAGS) -o $(NAME)$(SUFFIX) $(XLIBS) $(EXTRA_LIBS) \
		-lX11 -lXext
	mv xkegs ..

# Cygwin for X builds:
kegs.exe: $(OBJECTS) $(OBJECTS1) compile_time.o
	$(CC) $(CCOPTS) $(LDOPTS) $(OBJECTS) $(OBJECTS1) compile_time.o \
		$(LDFLAGS) -o $(NAME)$(SUFFIX) $(XLIBS) $(EXTRA_LIBS) \
		-lXext -lX11 -lm
	mv kegs.exe ..

# Mingw32 (native windows) builds: (broken, doesn't work currently)
kegswin.exe: $(OBJECTS) $(OBJECTS1) compile_time.o
	$(CC) $(CCOPTS) $(LDOPTS) $(OBJECTS) $(OBJECTS1) compile_time.o \
		$(LDFLAGS) -o $(NAME)$(SUFFIX) $(EXTRA_LIBS) \
		-lwinmm -lgdi32 -ldsound -lcomctl32 -lws2_32
	mv $(NAME)$(SUFFIX) ..


.s.o:
	$(AS) -c $(OPTS) -I. $*.s

.c.o:
	$(CC) $(CCOPTS) $(XOPTS) -c $(OPTS) -I. $*.c

.m.o:
	$(CC) $(CCOPTS) $(XOPTS) -c $(OPTS) -I. $*.m

AppDelegate.o: AppDelegate.swift
	sh ./comp_swift -c $(OPTS) -I. -primary-file $*.swift \
		MainView.swift -o $*.o

MainView.o: MainView.swift
	sh ./comp_swift -c $(OPTS) -I. -primary-file $*.swift \
		AppDelegate.swift -o $*.o

win32.o: win32.rc
	windres -o win32.o win32.rc

.c.proto:
	$(KMKROOT)/bin/kmkproto $(XOPTS) $*.c > $*.proto
	echo >> $*.proto

.m.proto:
	$(KMKROOT)/bin/kmkproto $(XOPTS) $*.m > $*.proto
	echo >> $*.proto

$(PROTO_OUT): $(PROTO_FILE_LIST) ldvars proto_vars
	$(KMKROOT)/bin/kmkproto_head $(PROTO_OUT) tmp_protos.h
	cat /dev/null $(PROTO_FILE_LIST) >> tmp_protos.h
	rm -f $(PROTO_OUT)
	mv tmp_protos.h $(PROTO_OUT)
	kmk_cp_if_diff $(PROTO_OUT) ..
	rm -f *.proto


compile_time.o: $(OBJECTS) $(OBJECTS1)

include dependency

