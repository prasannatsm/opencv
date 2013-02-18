# ----------------------------------------------------------------------------
#  CMake file for libwebp. See root CMakeLists.txt
#
# ----------------------------------------------------------------------------
project(${WEBP_LIBRARY})
#project(webp C)

ocv_include_directories(${CMAKE_CURRENT_SOURCE_DIR})

#file(GLOB lib_srcs *.c)
#file(GLOB lib_hdrs *.h)

FILE(GLOB WEBP_DEC_SRCS
	RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/dec/*c
)

FILE(GLOB WEBP_DSP_SRCS
	RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/dsp/*c
)

FILE(GLOB WEBP_ENC_SRCS
	RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/enc/*c
)

# ----------------------------------------------------------------------------------
#         Define the library target:
# ----------------------------------------------------------------------------------
add_library(${WEBP_LIBRARY} STATIC ${WEBP_DEC_SRCS} ${WEBP_DSP_SRCS} ${WEBP_ENC_SRCS})

if(UNIX)
  if(CMAKE_COMPILER_IS_GNUCXX OR CV_ICC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
    # set(CMAKE_C_FLAGS ${CMAKE_C_FLAGS} -fPIC)
	set(CMAKE_CXX_FLAGS ${CMAKE_cxx_FLAGS} -fPIC)
    set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} -Wall)
  endif()
endif()

ADD_DEFINITIONS(-DNDEBUG -DWEBP_USE_THREAD)

ocv_warnings_disable(CMAKE_C_FLAGS -Wcast-align -Wshadow -Wunused)
ocv_warnings_disable(CMAKE_C_FLAGS -Wunused-parameter) # clang

set_target_properties(${WEBP_LIBRARY}
  PROPERTIES OUTPUT_NAME ${WEBP_LIBRARY}
  #PROPERTIES OUTPUT_NAME "libwebp"
  DEBUG_POSTFIX "${OPENCV_DEBUG_POSTFIX}"
  ARCHIVE_OUTPUT_DIRECTORY ${3P_LIBRARY_OUTPUT_PATH}
  )

set(WEBP_LIBRARIES ${WEBP_LIBRARY})

target_link_libraries(libwebp m pthread)

if(ENABLE_SOLUTION_FOLDERS)
  set_target_properties(${WEBP_LIBRARY} PROPERTIES FOLDER "3rdparty")
endif()

if(NOT BUILD_SHARED_LIBS)
  install(TARGETS ${WEBP_LIBRARY} ARCHIVE DESTINATION ${OPENCV_3P_LIB_INSTALL_PATH} COMPONENT main)
endif()



#set(CMAKE_ALLOW_LOOSE_LOOP_CONSTRUCTS ON) - NOT DONE

#set(CMAKE_C_FLAGS ${CMAKE_C_FLAGS} "-O3"
#	-Wextra -Wold-style-definition 
#	-Wmissing-prototypes
#	-Wmissing-declarations
#	-Wdeclaration-after-statement
#)
