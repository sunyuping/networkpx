iPhoneDevPath=/Developer/Platforms/iPhoneOS.platform/Developer
iPhoneSDKPath=$(iPhoneDevPath)/SDKs/iPhoneOS3.0.sdk
MobileSubstratePath=../deb/Library/MobileSubstrate/DynamicLibraries
ThemesPath=../deb/Library/GriP/Themes
SharedSrcPath=../../src

Compiler=$(iPhoneDevPath)/usr/bin/gcc-4.2
CodeSign=ldid -S

Options=-arch armv6 \
	-std=c99 \
	-isysroot $(iPhoneSDKPath) \
	-F$(iPhoneSDKPath)/System/Library/PrivateFrameworks \
	-I$(iPhoneDevPath)/usr/include/gcc/darwin/default \
	-I$(iPhoneSDKPath)/usr/lib/gcc/arm-apple-darwin9/4.2.1/include \
	-I../../include \
	-L/usr/local/lib \
	-Wall \
	-L../deb/usr/lib \
	-mcpu=arm1176jzf-s \
	-O2

libGriP_Sources=Duplex/Client.o GPMessageWindow.o GPPreferences.o GPGetSmallAppIcon.o GPApplicationBridge.o
libGriP_Target=../deb/usr/lib/libGriP.dylib
libGriP_Options=-lobjc -dynamiclib -install_name /usr/lib/libGriP.dylib -framework Foundation -framework CoreFoundation -framework UIKit -framework CoreGraphics

SBHooker_GriP_Sources=SBHooker-GriP.o Duplex/Server.o
SBHooker_GriP_Target=$(MobileSubstratePath)/SBHooker-GriP.dylib
SBHooker_GriP_Options=-lobjc -lGriP -dynamiclib -framework Foundation -framework CoreFoundation -init _initialize

DefaultTheme_Sources=GPDefaultTheme.o
DefaultTheme_Target=$(ThemesPath)/Default.griptheme/Default
DefaultTheme_Options=-lobjc -bundle -lGriP -framework Foundation -framework CoreFoundation -framework UIKit -framework CoreGraphics

GriPTest_Sources=GriPTest.o GrowlApplicationBridge.o Duplex/Client.o GPApplicationBridge.o
GriPTest_Target=GriPTest
GriPTest_Options=-framework Foundation -framework CoreFoundation -framework UIKit -framework CoreGraphics 

# NotificationTestSources=test-notif.o NSDistributedNotificationCenter.o
# NotificationTestTarget=test-notif

all: $(libGriP_Target) $(DefaultTheme_Target) $(SBHooker_GriP_Target) $(GriPTest_Target)

%.o:	%.m
	$(Compiler) -c $(Options) -o $@ $^

%.o:	%.c
	$(Compiler) -c $(Options) -o $@ $^

$(libGriP_Target):	$(libGriP_Sources)
	$(Compiler) $(Options) $(libGriP_Options) -o $@ $^
	$(CodeSign) $@

$(SBHooker_GriP_Target):	$(SBHooker_GriP_Sources)
	$(Compiler) $(Options) $(SBHooker_GriP_Options) -o $@ $^
	$(CodeSign) $@

$(DefaultTheme_Target):	$(DefaultTheme_Sources)
	$(Compiler) $(Options) $(DefaultTheme_Options) -o $@ $^
	$(CodeSign) $@

$(GriPTest_Target):	$(GriPTest_Sources)
	$(Compiler) $(Options) $(GriPTest_Options) -o $@ $^
	$(CodeSign) $@

# $(NotificationTestTarget):	$(NotificationTestSources)
#	$(Compiler) $(Options) -framework Foundation -framework CoreFoundation -o $@ $^
#	$(CodeSign) $@

clean:
	rm -f *.o
	rm -f Duplex/*.o