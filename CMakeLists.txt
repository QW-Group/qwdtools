cmake_minimum_required(VERSION 3.4.0)

set(CMAKE_VERBOSE_MAKEFILE ON)

# Set project name and languge.
project(qwdtools C)


######################################################################################################

# Set where sources located.
set(DIR_SRC "src")

# Add sources
set(SRC_COMMON
	"${DIR_SRC}/bothtools.c"
	"${DIR_SRC}/dem_parse.c"
	"${DIR_SRC}/dem_send.c"
	"${DIR_SRC}/ini.c"
	"${DIR_SRC}/init.c"
	"${DIR_SRC}/main.c"
	"${DIR_SRC}/marge.c"
	"${DIR_SRC}/qwz.c"
	"${DIR_SRC}/sync.c"
	"${DIR_SRC}/tools.c"
	)


######################################################################################################

# Set base compiler flags
set(CFLAGS -Wall)
set(LFLAGS)


######################################################################################################

# Set target
add_executable(${PROJECT_NAME} ${SRC_COMMON})
set_target_properties(${PROJECT_NAME}
	PROPERTIES #PREFIX "" # Strip lib prefix.
	C_VISIBILITY_PRESET hidden # Hide all symbols unless excplicitly marked to export.
	)


######################################################################################################

# Set include directories
target_include_directories(${PROJECT_NAME} PRIVATE)


######################################################################################################

# Check build target, and included sources and libs
if(UNIX)
	target_link_libraries(${PROJECT_NAME} m)
	target_link_libraries(${PROJECT_NAME} dl)
else()
	target_link_libraries(${PROJECT_NAME} ws2_32)
	target_link_libraries(${PROJECT_NAME} winmm)
	set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc")
endif()


######################################################################################################

# Set defines for the build
target_compile_definitions(${PROJECT_NAME} PRIVATE SERVERONLY)
target_compile_definitions(${PROJECT_NAME} PRIVATE USE_PR2)

include (TestBigEndian)
TEST_BIG_ENDIAN(IS_BIG_ENDIAN)
if(IS_BIG_ENDIAN)
	target_compile_definitions(${PROJECT_NAME} __BIG_ENDIAN__Q__)
	message(STATUS "BIG_ENDIAN")
else()
	target_compile_definitions(${PROJECT_NAME} PRIVATE __LITTLE_ENDIAN__Q__)
	message(STATUS "LITTLE_ENDIAN")
endif()


######################################################################################################

# Assign compiler flags
target_compile_options(${PROJECT_NAME} PRIVATE ${CFLAGS})


######################################################################################################
