# Include CS SDK
CSSDK 		= include/cssdk

# Include Metamod SDK
METAMOD 	= include/metamod

# Release file output name
NAME 		= matchbot

# Release directory
BIN_DIR 	= Release

# G++
COMPILER 	= g++
OBJECTS 	= *.cpp

# Linker Libs
LINKER 		= -lcurl -lssl -lcrypto -ldl -lm -lz

# Compiler version check to static linking
RES_COMPILER_VERSION := $(shell expr "`$(COMPILER) -dumpversion`" ">=" "5")

# Link static lib if compiler is newer than minimum version
ifeq "$(RES_COMPILER_VERSION)" "1"
LINKER += -static-libstdc++
endif

# C flags
CFLAGS 		= -std=gnu++11 -pipe -g0 -O3 -s -flto -funroll-loops -fno-stack-protector -g -DNDEBUG -Dlinux -D__linux__ -D_GLIBCXX_USE_CXX11_ABI=0 -DCURL_STATICLIB -DHTTP_ONLY -m32 -msse3 -msse4.1 -shared

# Include directory
INCLUDE 	= -I. -I$(CSSDK)/common -I$(CSSDK)/dlls -I$(CSSDK)/engine -I$(CSSDK)/game_shared -I$(CSSDK)/pm_shared -I$(CSSDK)/public -I$(METAMOD)

# Linux objects to build
OBJ_LINUX 	:= $(OBJECTS:%.c=$(BIN_DIR)/%.o)

# Objects output path
$(BIN_DIR)/%.o: %.c

# Compile command
	$(COMPILER) $(INCLUDE) $(CFLAGS) -o $@ -c $<

all:
	mkdir -p $(BIN_DIR)

	$(MAKE) $(NAME) && strip -x $(BIN_DIR)/$(NAME)_mm.so

$(NAME): $(OBJ_LINUX)

	$(COMPILER) $(INCLUDE) $(CFLAGS) $(OBJ_LINUX) $(LINKER) -o$(BIN_DIR)/$(NAME)_mm.so

check:
	cppcheck $(INCLUDE) --quiet --max-configs=100 --std=gnu++11 -DNDEBUG -Dlinux -D__linux__ -U_FORTIFY_SOURCE .

default: all

clean:
	rm -rf Release/*.o
	rm -rf Release/*.so