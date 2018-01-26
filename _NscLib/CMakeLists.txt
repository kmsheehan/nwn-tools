add_library(nsc
	Nsc
	NscCodeGenerator
	NscCompiler
	NscContext
	NscDecompiler
	NscParserRoutines
	NscPStackEntry
	NscParser
)

if(MSVC)
	add_custom_command(
		OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/NscParser.cpp ${CMAKE_CURRENT_SOURCE_DIR}/NscParser.hpp
		DEPENDS NscParser.ypp
		COMMAND ${BISON_EXECUTABLE} -S lalr1.cc ${CMAKE_CURRENT_SOURCE_DIR}/NscParser.ypp -o ${CMAKE_CURRENT_SOURCE_DIR}/NscParser.cpp
	)
else()
	add_custom_command(
		OUTPUT NscParser.cpp NscParser.hpp
		DEPENDS NscParser.ypp
		COMMAND ${BISON_EXECUTABLE} -S lalr1.cc NscParser.ypp -o NscParser.cpp
	)
endif()
