# -- CMake build rules

CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT(Hello-world)

SET(SRC_LIST
	main.cpp
)

ADD_EXECUTABLE(${PROJECT_NAME} ${SRC_LIST})

#	MESSAGE("CMAKE_C_PLATFORM_ID is "	${CMAKE_C_PLATFORM_ID})

if("${CMAKE_C_PLATFORM_ID}" MATCHES "Windows")
	if(MSVC)
		MESSAGE(STATUS "Entering Windows/MSVC branch.")
		SET(CMAKE_CXX_FLAGS
			"${CMAKE_CXX_FLAGS} -D_SCL_SECURE_NO_WARNINGS /W4 /WX /EHa /wd4351")
		# '- http://msdn.microsoft.com/en-US/library/thxezb7y.aspx:
		#		  /wd n  Disables the compiler warning that is specified in n.
		#			For example, /wd4326 disables compiler warning C4326.
		SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /FORCE:MULTIPLE")
		SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /LARGEADDRESSAWARE")
		MESSAGE(STATUS "CMAKE_EXE_LINKER_FLAGS are: " ${CMAKE_EXE_LINKER_FLAGS})
	else()
		MESSAGE(ERROR "Uncontemplated case.")
	endif()
elseif("${CMAKE_C_PLATFORM_ID}" MATCHES "Linux")
	if(CMAKE_COMPILER_IS_GNUCXX)
		MESSAGE(STATUS "Entering Linux/CMAKE_COMPILER_IS_GNUCXX branch.")
		SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
	else()
		MESSAGE(STATUS "Entering Linux/else branch. We suppose clang++.")
		SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
	endif()
elseif(“${CMAKE_C_PLATFORM_ID}” MATCHES “Darwin”)
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
else()
	MESSAGE(ERROR “Unknown system”)
endif()

MESSAGE(STATUS "CMAKE_CXX_FLAGS are:" ${CMAKE_CXX_FLAGS})

# Link the executable to the required libraries

TARGET_LINK_LIBRARIES(${PROJECT_NAME}
	)
#	${Boost_LIBRARIES})
#	${Boost_LIBRARIES} ${OpenIGTLink_LIBRARIES})

# -- eof
