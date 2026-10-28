file(GLOB _SOURCES "src/${RUN_ARCH}/*.c")

if (CONFIG_FAST_IMAGE_TYPE STRGREATER "0")
add_compile_definitions(FAST_IMAGE_ENABLE)
endif()

include_directories(include)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/arch)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/common)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/kernel)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/driver/spinlock)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/driver/jenc)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/driver/rtos_cmdqu)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/driver/fast_image)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/hal/config)

add_library(comm STATIC ${_SOURCES})
install(TARGETS comm DESTINATION lib)


# TideSense debug features are explicitly enabled by scripts/c906/build.sh.
option(TIDESENSE_C906_RELOAD "Enable cooperative C906 firmware RAM reload" OFF)
option(TIDESENSE_C906_TPU_PROBE "Enable standalone C906 TPU access experiment" OFF)
if(TIDESENSE_C906_RELOAD)
    target_compile_definitions(comm PRIVATE TIDESENSE_C906_RELOAD)
endif()
if(TIDESENSE_C906_TPU_PROBE)
    target_compile_definitions(comm PRIVATE TIDESENSE_C906_TPU_PROBE)
endif()
