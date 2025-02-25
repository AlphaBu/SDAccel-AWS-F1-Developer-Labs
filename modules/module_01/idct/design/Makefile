.PHONY: help

help::
	@echo  " Makefile Usage:"
	@echo  ""
	@echo  "  make build TARGET=<sw_emu/hw_emu/hw> "
	@echo  "  Command to generate the design for specified target"
	@echo  ""
	@echo  "  make run TARGET=<sw_emu/hw_emu/hw> "
	@echo  "  Command to generate and run the design for specified target"
	@echo  ""
	@echo  "  make clean TARGET=<sw_emu/hw_emu/hw> "
	@echo  "  Command to remove the generated non-hardware files for specified target"
	@echo  ""


## platform selection
## Several supported platforms are liste here.
## Or you can add other platfom and set PLATFORM variable to it

PLATFORM := $(AWS_PLATFORM)


## TARGET can be set as:
## sw_emu: software emulation
## hw_emu: hardware Emulation
## hw: hardware run
TARGET := sw_emu

## Below are the names for host executable and xclbin.
## Please keep it unchanged.
HOST_EXE := IDCT.exe
XO_NAME := krnl_idct
XCLBIN := krnl_idct_$(TARGET).xclbin

SRC_REPO := ./src
BUILD_DIR := ./build


## Host Application files repository

HOST_SRC_CPP := $(SRC_REPO)/idct.cpp

## Kernel Source Files repository

KERNEL_SRC_CPP := $(SRC_REPO)/krnl_idct.cpp

# Host Compiler Global Settings and Include Libraries

CXXFLAGS += -D__USE_XOPEN2K8
CXXFLAGS += -I$(XILINX_XRT)/include/
CXXFLAGS += -I$(XILINX_VIVADO)/include/
CXXFLAGS += -I$(SRC_REPO)
CXXFLAGS += -O0 -g -Wall -fmessage-length=0 -std=c++14

CXXLDFLAGS := -L$(XILINX_XRT)/lib/
CXXLDFLAGS += -lxilinxopencl -lpthread -lrt


## Kernel Compiler and Linker Flags

XOCCFLAGS := -t $(TARGET)
XOCCFLAGS += --platform $(PLATFORM) -R1 --save-temps
XOCCFLAGS += --nk krnl_idct:1
XOCCFLAGS += --temp_dir $(BUILD_DIR)
XOCCFLAGS += --log_dir $(BUILD_DIR)
XOCCFLAGS += --profile_kernel data:all:all:all:all
XOCCFLAGS += --sp krnl_idct_1.m_axi_gmem:bank0 --sp krnl_idct_1.m_axi_gmem1:bank0 --sp krnl_idct_1.m_axi_gmem2:bank1

## Host Executable File Generation

$(BUILD_DIR)/$(HOST_EXE): $(HOST_SRC_CPP)
	mkdir -p $(BUILD_DIR)
	xcpp $(CXXFLAGS) $(CXXLDFLAGS) -o $@ $(HOST_SRC_CPP)

## Kernel XO and Xclbin File Generation

$(BUILD_DIR)/$(XO_NAME).xo: $(KERNEL_SRC_CPP)
	mkdir -p $(BUILD_DIR)
	xocc $(XOCCFLAGS) -c -k krnl_idct $(KERNEL_SRC_CPP) -o $@

$(BUILD_DIR)/$(XCLBIN): $(BUILD_DIR)/$(XO_NAME).xo
	mkdir -p $(BUILD_DIR)
	xocc $(XOCCFLAGS) -l -o $@ $(BUILD_DIR)/$(XO_NAME).xo

## Emulation Files Generation

EMCONFIG_FILE = emconfig.json

$(BUILD_DIR)/$(EMCONFIG_FILE):
	 emconfigutil --nd 1  --platform $(PLATFORM) --od $(BUILD_DIR)

# primary build targets

.PHONY: all clean

## build the design without running host application

build: $(BUILD_DIR)/$(HOST_EXE) $(BUILD_DIR)/$(XCLBIN) $(BUILD_DIR)/$(EMCONFIG_FILE)

## build the design and then run host application

run: build
	cp sdaccel.ini $(BUILD_DIR);
ifeq ($(TARGET), hw)
	cd $(BUILD_DIR) && unset XCL_EMULATION_MODE;    ./IDCT.exe  --kernel_name krnl_idct ./$(XCLBIN) ;
else
	cd $(BUILD_DIR) && XCL_EMULATION_MODE=$(TARGET) ./IDCT.exe  --kernel_name krnl_idct ./$(XCLBIN) ;
endif

## Clean generated files

clean:
	rm -rf $(BUILD_DIR)/$(XCLBIN) $(BUILD_DIR)/$(HOST_EXE) $(BUILD_DIR)/$(EMCONFIG_FILE) $(BUILD_DIR)/$(XO_NAME).xo $(BUILD_DIR)/*.ltx $(BUILD_DIR)/*_$(TARGET).log $(BUILD_DIR)/xocc_*_$(TARGET)_* $(BUILD_DIR)/_x* $(BUILD_DIR)/*.info $(BUILD_DIR)/convolve_fpga_$(TARGET)* $(BUILD_DIR)/link $(BUILD_DIR)/reports/convolve_fpga_$(TARGET)
