CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT (pyilmbase)

SET(ILMBASE_VERSION_MAJOR "2")
SET(ILMBASE_VERSION_MINOR "2")
SET(ILMBASE_VERSION_PATCH "0")

SET(ILMBASE_VERSION ${ILMBASE_VERSION_MAJOR}.${ILMBASE_VERSION_MINOR}.${ILMBASE_VERSION_PATCH})
SET(ILMBASE_VERSION_API ${ILMBASE_VERSION_MAJOR}_${ILMBASE_VERSION_MINOR})

message("========================= in contrib/PyIlmBase-2.2.0/CMakeLists.txt =========================")
set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${DISABLE_WARNING}")
message("CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS}")

# enable the tests
ENABLE_TESTING()

# distro building 
SET(CPACK_PACKAGE_VERSION_MAJOR "${ILMBASE_VERSION_MAJOR}")
SET(CPACK_PACKAGE_VERSION_MINOR "${ILMBASE_VERSION_MINOR}")
SET(CPACK_PACKAGE_VERSION_PATCH "${ILMBASE_VERSION_PATCH}")
SET(CPACK_SOURCE_GENERATOR "TGZ")
set(CPACK_SOURCE_PACKAGE_FILE_NAME
    "${CMAKE_PROJECT_NAME}-${ILMBASE_VERSION}"
  )
set(CPACK_SOURCE_IGNORE_FILES
  "/.git*;/.cvs*;${CPACK_SOURCE_IGNORE_FILES}")
INCLUDE ( CPack )

# Set this option to find the C++ libs with their namespaced versions
# The python modules themselves are built without versioning
OPTION (NAMESPACE_VERSIONING "Use Namespace Versioning" ON)

# Setup osx rpathing
SET (CMAKE_MACOSX_RPATH 1)
SET (BUILD_WITH_INSTALL_RPATH 1)

SET (LIB_TYPE SHARED)

# add boost include
set(Boost_DEBUG 1)
set(CMAKE_INCLUDE_PATH ${CMAKE_INCLUDE_PATH} )
set(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} )
add_definitions(-DBOOST_ALL_DYN_LINK)

FIND_PACKAGE ( PythonLibs REQUIRED )
FIND_PACKAGE ( PythonInterp REQUIRED )
FIND_PACKAGE ( Boost
	COMPONENTS Python
)
FIND_PACKAGE ( NumPy )

INCLUDE_DIRECTORIES (
	${ILMBASE_PACKAGE_PREFIX}/include/OpenEXR
	PyIex
	PyImath
	PyImathNumpy
	${PYTHON_INCLUDE_PATH}
	${CMAKE_INCLUDE_PATH}
)

LINK_DIRECTORIES ( ${ILMBASE_PACKAGE_PREFIX}/lib 
				   ${CMAKE_LIBRARY_PATH}
				 )
MESSAGE ( "ILMBASE_PACKAGE_PREFIX = " ${ILMBASE_PACKAGE_PREFIX})

if(WIN32)
	add_definitions(-DPLATFORM_WINDOWS)
endif()

SET (ILMBASE_LIBSUFFIX "")
IF (NAMESPACE_VERSIONING)
  SET ( ILMBASE_LIBSUFFIX "-${ILMBASE_VERSION_API}" )
ENDIF ()

##########################
ADD_SUBDIRECTORY ( PyIex )
ADD_SUBDIRECTORY ( PyImath )

IF (NUMPY_FOUND)
  ADD_SUBDIRECTORY ( PyImathNumpy )
  ADD_SUBDIRECTORY ( PyImathNumpyTest )
ENDIF ()
  
##########################
# Tests
##########################
ADD_SUBDIRECTORY ( PyIexTest )
ADD_SUBDIRECTORY ( PyImathTest )

