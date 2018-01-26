#add_library(docopt
#        docopt
#        )

##============================================================================
## Sources & headers
##============================================================================
set(docopt_SOURCES docopt.cpp)
set(docopt_HEADERS
        docopt.h
        docopt_private.h
        docopt_util.h
        docopt_value.h
        )

if(MSVC OR XCODE)
    # MSVC requires __declspec() attributes, which are achieved via the
    # DOCOPT_DLL and DOCOPT_EXPORTS macros below. Since those macros are only
    # defined when building a shared library, we must build the shared and
    # static libraries completely separately.
    # Xcode does not support libraries with only object files as sources.
    # See https://cmake.org/cmake/help/v3.0/command/add_library.html?highlight=add_library
    #add_library(docopt SHARED ${docopt_SOURCES} ${docopt_HEADERS})
    add_library(docopt_s STATIC ${docopt_SOURCES} ${docopt_HEADERS})
else()
    # If not using MSVC or Xcode, we will create an intermediate object target
    # to avoid compiling the source code twice.
    add_library(docopt_o OBJECT ${docopt_SOURCES} ${docopt_HEADERS})
    set_target_properties(docopt_o PROPERTIES POSITION_INDEPENDENT_CODE TRUE)

    add_library(docopt SHARED $<TARGET_OBJECTS:docopt_o>)
    add_library(docopt_s STATIC $<TARGET_OBJECTS:docopt_o>)
endif()

if(MSVC)
    # DOCOPT_DLL: Must be specified when building *and* when using the DLL.
    #             That's what the "PUBLIC" means.
    # DOCOPT_EXPORTS: Must use __declspec(dllexport) when building the DLL.
    #                 "PRIVATE" means it's only defined when building the DLL.
    target_compile_definitions(docopt PUBLIC  DOCOPT_DLL
            PRIVATE DOCOPT_EXPORTS)
endif()

if(NOT MSVC)
    set_target_properties(docopt PROPERTIES OUTPUT_NAME docopt)
    set_target_properties(docopt_s PROPERTIES OUTPUT_NAME docopt)
endif()
