# makefile for arm-linux
#   gcc version 4

CC		=	gcc
CXX		=   g++
STRIP	=	strip
RM		=	rm -f

DBX_PATH	= ..

DYNAMIC_X86 = 1

OPENGL = 1

CXXFLAGS = -O3  -DNDEBUG \
			-std=gnu++98 -Wstrict-overflow=0 -Wstrict-aliasing=0 -fomit-frame-pointer  -mno-ms-bitfields  -I. \
			-I$(DBX_PATH) \
			-I$(DBX_PATH)/include \
			-I$(DBX_PATH)/src \
			-I$(DBX_PATH)/src/aviwriter \
			-I$(DBX_PATH)/src/hardware/snd_pc98/sound \
			-I$(DBX_PATH)/src/hardware/snd_pc98/common \
			-I$(DBX_PATH)/src/hardware/snd_pc98/generic \
			-I$(DBX_PATH)/src/hardware/snd_pc98/sound/getsnd \
			-I$(DBX_PATH)/src/hardware/snd_pc98/x11 \
			-I$(DBX_PATH)/src/hardware/snd_pc98/cbus 
			
ifeq ($(SVN_DYNREC), 1)
	CXXFLAGS	+=  -DC_DYNREC=1
	DYNAMIC_X86 = 0
endif

ifeq ($(DYNAMIC_X86), 1)
	CXXFLAGS	+=  -DC_DYNAMIC_X86=32
endif

CFLAGS	=	-O3 -DNDEBUG -Wstrict-overflow=0 -Wstrict-aliasing=0 -fomit-frame-pointer  -mno-ms-bitfields  -I. \
			-I$(DBX_PATH) \
			-I$(DBX_PATH)/include \
			-I$(DBX_PATH)/src \
			-I$(DBX_PATH)/src/aviwriter \
			-I$(DBX_PATH)/src/hardware/snd_pc98/sound \
			-I$(DBX_PATH)/src/hardware/snd_pc98/common \
			-I$(DBX_PATH)/src/hardware/snd_pc98/generic \
			-I$(DBX_PATH)/src/hardware/snd_pc98/sound/getsnd \
			-I$(DBX_PATH)/src/hardware/snd_pc98/x11 \
			-I$(DBX_PATH)/src/hardware/snd_pc98/cbus 
			
ifeq ($(SDL1), 1)
	CFLAGS	+=  $(shell sdl-config --cflags)
	CXXFLAGS += $(shell sdl-config --cflags)
else
	CFLAGS	+=	-DC_SDL2=1 $(shell sdl2-config --cflags)
	CXXFLAGS += -DC_SDL2=1 $(shell sdl2-config --cflags)
endif

ifeq ($(OPENGL), 1)
	CFLAGS	+= -DC_OPENGL=1
	CXXFLAGS += -DC_OPENGL=1
endif
	
LFLAGS	=

TARGET	=	dosbox-x.exe

CPPSRCS	=	$(DBX_PATH)/src/dosbox.cpp \
			$(wildcard $(DBX_PATH)/src/aviwriter/*.cpp) \
			$(wildcard $(DBX_PATH)/src/builtin/*.cpp) \
			$(wildcard $(DBX_PATH)/src/cpu/*.cpp) \
			$(wildcard $(DBX_PATH)/src/debug/*.cpp) \
			$(wildcard $(DBX_PATH)/src/dos/*.cpp) \
			$(wildcard $(DBX_PATH)/src/fpu/*.cpp) \
			$(wildcard $(DBX_PATH)/src/gui/*.cpp) \
			$(wildcard $(DBX_PATH)/src/gui/*.c) \
			$(wildcard $(DBX_PATH)/src/hardware/*.cpp) \
			$(wildcard $(DBX_PATH)/src/hardware/parport/*.cpp) \
			$(wildcard $(DBX_PATH)/src/hardware/reSID/*.cpp) \
			$(wildcard $(DBX_PATH)/src/hardware/serialport/*.cpp) \
			$(DBX_PATH)/src/hardware/snd_pc98/cbus/board86.c \
			$(DBX_PATH)/src/hardware/snd_pc98/cbus/board26k.c \
			$(wildcard $(DBX_PATH)/src/hardware/snd_pc98/common/*.c) \
			$(wildcard $(DBX_PATH)/src/hardware/snd_pc98/generic/*.c) \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/opngeng.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/opngenc.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/pcm86g.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/pcm86c.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/tms3631g.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/tms3631c.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/psggeng.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/psggenc.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/adpcmg.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/adpcmc.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/rhythmc.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/fmboard.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/fmtimer.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/sound.c \
			$(DBX_PATH)/src/hardware/snd_pc98/sound/soundrom.c \
			$(wildcard $(DBX_PATH)/src/hardware/snd_pc98/sound/getsnd/*.c) \
			$(wildcard $(DBX_PATH)/src/hardware/snd_pc98/x11/*.c) \
			$(wildcard $(DBX_PATH)/src/ints/*.cpp) \
			$(wildcard $(DBX_PATH)/src/libs/gui_tk/*.cpp) \
			$(wildcard $(DBX_PATH)/src/misc/*.cpp) \
			$(wildcard $(DBX_PATH)/src/shell/*.cpp) 

CPPSRCS_MT32 = $(wildcard $(DBX_PATH)/src/mt32/*.cpp) \
			$(wildcard $(DBX_PATH)/src/mt32/freeverb/*.cpp) \
			$(wildcard $(DBX_PATH)/src/mt32/sha1/*.cpp)

OBJS_MT32 = $(addsuffix .o,$(basename $(CPPSRCS_MT32)))

CXXFLAGS_MT32 = -DC_MT32=1 -I$(DBX_PATH)/src/mt32 -I$(DBX_PATH)/src/mt32/freeverb -I$(DBX_PATH)/src/mt32/sha1
	
OBJS = $(addsuffix .o,$(basename $(CPPSRCS)))
LIBS = -static -lz -lpng -lm -lstdc++ 

ifeq ($(SDL1), 1) 
	LIBS += $(shell sdl-config --libs)
	ifdef WINDIR
		LIBS += -limm32
	endif
else
	LIBS += $(shell sdl2-config --libs)
endif

ifeq ($(OPENGL), 1)
	ifdef WINDIR
		LIBS += -lopengl32
	else
		LIBS += -lGL
	endif
endif
	
ifeq ($(MT32),1)
	CPPSRCS += $(CPPSRCS_MT32)
	CXXFLAGS += $(CXXFLAGS_MT32)
#	OBJS += $(OBJS_MT32)
endif

.SUFFIXES: .c.o
.SUFFIXES: .cpp.o

all:	$(TARGET)

$(TARGET):	$(OBJS)
	$(CC) $(LFLAGS) -g -o $@ $(OBJS) $(LIBS)
	$(STRIP) $@

.c.o:
	$(CC) $(CFLAGS) -c -o $@ $<

.cpp.o:
	$(CXX) $(CXXFLAGS) -c -o $@ $<

clean:
	rm -f $(TARGET) $(OBJS) $(OBJS_MT32)
