MODEL ?= Z10

CXX=$(CROSS_COMPILE)g++
RM=rm

PROJECT_ROOT=../..

CXXSOURCES=main.cpp

CXXSOURCES_SERVICE=service.cpp

INCLUDE =  -I$(INSTALL_DIR)/include
INCLUDE += -I$(INSTALL_DIR)/include/api2
INCLUDE += -I$(INSTALL_DIR)/include/apiApp
INCLUDE += -I$(INSTALL_DIR)/rp_sdk
INCLUDE += -I$(INSTALL_DIR)/rp_sdk/libjson

INCLUDE += -I./server/include/rpsa/server/core
INCLUDE += -I./server/include/rpsa/common/core
INCLUDE += -I./server/common
INCLUDE += -I./server/libs/asio/include


LIBS = -L$(INSTALL_DIR)/lib
LIBS += -L$(INSTALL_DIR)/rp_sdk
LIBS += -L./server/bin

COMMON_FLAGS+=-Wall -fPIC -Os -s -w -DASIO_STANDALONE -DASDASD 
CFLAGS += $(COMMON_FLAGS) $(INCLUDE)
CXXFLAGS+=$(COMMON_FLAGS) -std=c++11 $(INCLUDE) -D$(MODEL)
LDFLAGS = $(COMMON_FLAGS) $(LIBS)
LDWFLAGS+= -Wl,--whole-archive
LDWFLAGS+= -lrp_sdk -lrp
LDWFLAGS+= -lrpsasrv
LDWFLAGS+= -Wl,--no-whole-archive
LDWFLAGS+= -shared 

LDSFLAGS+= -Wl,--whole-archive
LDSFLAGS+= -lrpsasrv
LDSFLAGS+= -Wl,--no-whole-archive
LDSFLAGS+= -lpthread

COBJECTS=$(CSOURCES:.c=.o)
CXXOBJECTS=$(CXXSOURCES:.cpp=.o)
OBJECTS=$(COBJECTS) $(CXXOBJECTS)

CXXOBJECTS_SERVICE=$(CXXSOURCES_SERVICE:.cpp=.o)
OBJECTS_SERVICE=$(COBJECTS) $(CXXOBJECTS_SERVICE)

OUT_DIR=../
C_OUT_NAME=$(OUT_DIR)controllerhf.so
C_OUT_NAME_SERVICE=$(OUT_DIR)streaming-server

all: make_c_app service
clean: clean_c_app

build_lib:
	./build.sh libs

.PHONY: make_c_app
make_c_app: build_lib  $(OBJECTS)
	$(CXX) -o $(C_OUT_NAME) $(OBJECTS) $(CFLAGS) $(LDFLAGS) $(LDWFLAGS)

.PHONY: service
service: build_lib $(OBJECTS_SERVICE)
	$(CXX) -o $(C_OUT_NAME_SERVICE) $(OBJECTS_SERVICE) $(CFLAGS) $(LDFLAGS) $(LDSFLAGS)

clean_c_app:
	$(RM) -f $(C_OUT_NAME) $(OBJECTS)
	./build.sh clean
