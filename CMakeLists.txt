#find_package(Git QUIET)
#if(GIT_FOUND AND EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/.git")
## Update submodules as needed
#    option(GIT_SUBMODULE "Check submodules during build" ON)
#    if(GIT_SUBMODULE)
#        message(STATUS "Submodule update")
#        execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
#                        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
#                        RESULT_VARIABLE GIT_SUBMOD_RESULT)
#        if(NOT GIT_SUBMOD_RESULT EQUAL "0")
#            message(FATAL_ERROR "git submodule update --init failed with ${GIT_SUBMOD_RESULT}, please checkout submodules")
#        endif()
#    endif()
#endif()
#
#if(NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/vendor/breakpad/src")
#	message(STATUS "${CMAKE_CURRENT_SOURCE_DIR}/vendor/breakpad/src")
#    message(FATAL_ERROR "The submodules were not downloaded! GIT_SUBMODULE was turned off or failed. Please update submodules and try again.")
#endif()

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

# Find Qt libraries
find_package(Qt5 REQUIRED
    COMPONENTS
	    Core)
include_directories(${Qt5Core_INCLUDE_DIRS})

set(SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/qt_breakpad.cpp)
set(HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/qt_breakpad.h)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
set(GOOGLE_BREAKPAD_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/vendor/breakpad/src)
include_directories(${GOOGLE_BREAKPAD_SOURCE_DIR})

# Windows
if(WIN32)
    list(APPEND HEADERS
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/windows/string_utils-inl.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/windows/guid_string.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/windows/handler/exception_handler.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/windows/common/ipc_protocol.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/google_breakpad/common/minidump_format.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/google_breakpad/common/breakpad_types.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/windows/crash_generation/crash_generation_client.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/scoped_ptr.h
	)
    list(APPEND SOURCES
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/windows/handler/exception_handler.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/windows/string_utils.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/windows/guid_string.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/windows/crash_generation/crash_generation_client.cc
        )
elseif(UNIX)
    list(APPEND HEADERS
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/cpu_set.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/proc_cpuinfo_reader.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/handler/exception_handler.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/crash_generation/crash_generation_client.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/handler/minidump_descriptor.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/minidump_writer.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/line_reader.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/linux_dumper.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/linux_ptrace_dumper.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/directory_reader.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/log/log.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/minidump_file_writer-inl.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/minidump_file_writer.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/linux_libc_support.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/eintr_wrapper.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/ignore_ret.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/file_id.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/memory_mapped_file.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/safe_readlink.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/guid_creator.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/elfutils.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/elfutils-inl.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/elf_gnu_compat.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/using_std_string.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/memory.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/basictypes.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/memory_range.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/string_conversion.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/convert_UTF.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/google_breakpad/common/minidump_format.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/google_breakpad/common/minidump_size.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/google_breakpad/common/breakpad_types.h
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/scoped_ptr.h
	)
    list(APPEND SOURCES
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/crash_generation/crash_generation_client.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/handler/exception_handler.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/handler/minidump_descriptor.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/minidump_writer.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/linux_dumper.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/minidump_writer/linux_ptrace_dumper.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/linux/log/log.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/client/minidump_file_writer.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/linux_libc_support.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/file_id.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/memory_mapped_file.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/safe_readlink.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/guid_creator.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/linux/elfutils.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/string_conversion.cc
        ${GOOGLE_BREAKPAD_SOURCE_DIR}/common/convert_UTF.c
    )

endif()


## breakpad needs debug info inside binaries

if(WIN32)
    add_compile_definitions(UNICODE)
    if(MSVC)
        # generate the symbol file
        set(LDFLAGS "${LDFLAGS} /MAP /debug /opt:ref")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Zi")
        set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -Zi")
    endif()
elseif(UNIX)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g")
    set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -g")
endif()

# prevent undue optimization, which ruins breakpad's backtrace
list(REMOVE_ITEM CMAKE_C_FLAGS -O -O1 -O2)
list(REMOVE_ITEM CMAKE_CXX_FLAGS -O -O1 -O2)

add_library(qt-breakpad
    ${SOURCES})
