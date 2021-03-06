TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS BELOW HERE

# xxxRecord.h will be created from xxxRecord.dbd
DBDINC += xxxRecord

# Install xxxSupport.dbd into <top>/dbd
DBD += xxxSupport.dbd

# Build an IOC support library
LIBRARY_IOC += exampleSupport

# Compile and add the code to the support library
exampleSupport_SRCS += xxxRecord.c
exampleSupport_SRCS += devXxxSoft.c

# Link locally-provided code into the support library,
# rather than directly into the IOC application.
# This is required for Windows DLL builds.
exampleSupport_SRCS += dbSubExample.c
exampleSupport_SRCS += exampleHello.c
exampleSupport_SRCS += initTrace.c

exampleSupport_LIBS += $(EPICS_BASE_IOC_LIBS)


# Build the IOC application
PROD_IOC_RTEMS = example

# example.dbd will be created and installed
DBD += example.dbd

# example.dbd will include these files:
example_DBD += base.dbd
example_DBD += xxxSupport.dbd
example_DBD += dbSubExample.dbd
example_DBD += exampleHello.dbd
example_DBD += initTrace.dbd

# example_registerRecordDeviceDriver.cpp derives from example.dbd
example_SRCS += example_registerRecordDeviceDriver.cpp

# Build the main IOC entry point where needed
example_SRCS_DEFAULT += exampleMain.cpp
example_SRCS_vxWorks += -nil-

# Link in the code from our support library
example_LIBS += exampleSupport
example_LIBS += electricFence
LDFLAGS  += -Wl,--wrap,malloc -Wl,--wrap,realloc -Wl,--wrap,calloc -Wl,--wrap,free
LDFLAGS  += -Wl,--wrap,_malloc_r -Wl,--wrap,_realloc_r -Wl,--wrap,_calloc_r -Wl,--wrap,_free_r

# To build SNL programs, SNCSEQ must be defined
# in the <top>/configure/RELEASE file
ifneq ($(SNCSEQ),)
    # Build sncExample into exampleSupport
    sncExample_SNCFLAGS += +r
    example_DBD += sncExample.dbd
    # A .stt sequence program is *not* pre-processed:
    exampleSupport_SRCS += sncExample.stt
    exampleSupport_LIBS += seq pv
    example_LIBS += seq pv

    # Build sncProgram as a standalone program
    PROD_HOST += sncProgram
    sncProgram_SNCFLAGS += +m
    # A .st sequence program *is* pre-processed:
    sncProgram_SRCS += sncProgram.st
    sncProgram_LIBS += seq pv
    sncProgram_LIBS += $(EPICS_BASE_HOST_LIBS)
endif

# Finally link IOC to the EPICS Base libraries
example_LIBS += $(EPICS_BASE_IOC_LIBS)

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD EXTRA GNUMAKE RULES BELOW HERE

