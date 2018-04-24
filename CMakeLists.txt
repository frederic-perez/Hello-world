# -- CMake build rules

cmake_minimum_required(VERSION 2.8)

set(PROJECT_NAME "Hello-world")
set(PROJECT_TYPE "CXX")
project(${PROJECT_NAME} ${PROJECT_TYPE})

set(SRC_LIST
	main.cpp
)

add_executable(${PROJECT_NAME} ${SRC_LIST})

set(CMAKE_VERBOSE_MAKEFILE TRUE)

message(STATUS "CMAKE_C_PLATFORM_ID is ${CMAKE_C_PLATFORM_ID}")
message(STATUS "CMAKE_SYSTEM_NAME is ${CMAKE_SYSTEM_NAME}")

message(STATUS "CMAKE_GENERATOR is ${CMAKE_GENERATOR}")
message(STATUS "CMAKE_GENERATOR_TOOLSET is ${CMAKE_GENERATOR_TOOLSET}")

message(STATUS "CMAKE_CXX_COMPILER_ID is ${CMAKE_CXX_COMPILER_ID}")
message(STATUS "CMAKE_CXX_COMPILER is ${CMAKE_CXX_COMPILER}")
message(STATUS "CMAKE_CXX_COMPILER_VERSION is ${CMAKE_CXX_COMPILER_VERSION}")

if("${CMAKE_SYSTEM_NAME}" MATCHES "Windows")
	if(MSVC)
		message(STATUS "Entering Windows/MSVC branch.")
		if("${CMAKE_GENERATOR_TOOLSET}" MATCHES "LLVM-vs2014")
			message(STATUS "Entering Windows/MSVC/LLVM-vs2014 branch.")
			string(CONCAT CMAKE_CXX_FLAGS
				"-std=c++11 "
				"-Wall -Wextra -Wshadow -Wnon-virtual-dtor -Wold-style-cast -Wcast-align "
				"-Wunused -Woverloaded-virtual -Wpedantic -Wconversion -Wsign-conversion "
				"-Wnull-dereference "
				"-Wformat=2 "
				# "-Wmisleading-indentation -Wduplicated-cond -Wduplicated-branches "
				# "-Wlogical-op -Wuseless-cast "
				"-Wdouble-promotion ")
		else()
			message(STATUS "Entering Windows/MSVC/__not__LLVM-vs2014 branch.")
			string(CONCAT CMAKE_CXX_FLAGS
				"-D_SCL_SECURE_NO_WARNINGS -D_CRT_SECURE_NO_WARNINGS "
				"/W4 /WX /EHa /wd4351")
		endif()
		# '- http://msdn.microsoft.com/en-US/library/thxezb7y.aspx:
		#		  /wd n  Disables the compiler warning that is specified in n.
		#			For example, /wd4326 disables compiler warning C4351.
		set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /FORCE:MULTIPLE")
		set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /LARGEADDRESSAWARE")
		message(STATUS "CMAKE_EXE_LINKER_FLAGS are: " ${CMAKE_EXE_LINKER_FLAGS})
	else()
		message(STATUS "Entering Windows/__not__MSVC branch.")
		if(NOT CMAKE_CXX_COMPILER_ID)
			message(STATUS "Empty CMAKE_CXX_COMPILER_ID; we will assume Clang")
			set(CMAKE_CXX_COMPILER_ID "Clang")
		endif()
		if("${CMAKE_CXX_COMPILER_ID}" MATCHES "GNU")
			message(STATUS "Entering Windows/__not__MSVC/GNU branch.")
			string(CONCAT CMAKE_CXX_FLAGS
				"-std=c++11 "
				"-Wall -Wextra -Wshadow -Wnon-virtual-dtor -Wold-style-cast -Wcast-align "
				"-Wunused -Woverloaded-virtual -Wpedantic -Wconversion -Wsign-conversion "
				"-Wnull-dereference "
				"-Wformat=2 "
				# "-Wmisleading-indentation -Wduplicated-cond -Wduplicated-branches "
				# "-Wlogical-op -Wuseless-cast "
				"-Wdouble-promotion ")
		elseif("${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang")
			message(STATUS "Entering Windows/__not__MSVC/Clang branch.")
			string(CONCAT CMAKE_CXX_FLAGS
				"-std=c++11 "
				"-Wall -Wextra -Wshadow -Wnon-virtual-dtor -Wold-style-cast -Wcast-align "
				"-Wunused -Woverloaded-virtual -Wpedantic -Wconversion -Wsign-conversion "
				"-Wnull-dereference "
				"-Wformat=2 "
				# "-Wmisleading-indentation -Wduplicated-cond -Wduplicated-branches "
				# "-Wlogical-op -Wuseless-cast "
				"-Wdouble-promotion ")
		else()
			message(ERROR ": Uncontemplated CMAKE_CXX_COMPILER_ID case.")
		endif()
	endif()
elseif("${CMAKE_SYSTEM_NAME}" MATCHES "Linux")
	if(CMAKE_COMPILER_IS_GNUCXX)
		message(STATUS "Entering Linux/CMAKE_COMPILER_IS_GNUCXX branch.")
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
	else()
		message(STATUS "Entering Linux/else branch. We assume clang++.")
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
	endif()
elseif("${CMAKE_SYSTEM_NAME}" MATCHES "Darwin")
	message(STATUS "Entering Darwin branch. We assume clang++.")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
else()
	message(FATAL_ERROR ": Unknown system")
endif()

message(STATUS "CMAKE_CXX_FLAGS are:" ${CMAKE_CXX_FLAGS})

# Link the executable to the required libraries

target_link_libraries(${PROJECT_NAME}
#	${Boost_LIBRARIES}
#	${Boost_LIBRARIES} ${OpenIGTLink_LIBRARIES}
)

# -- eof
