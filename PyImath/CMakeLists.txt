add_definitions(-DPYIMATH_EXPORTS)

ADD_LIBRARY ( PyImath ${LIB_TYPE}
    PyImath.cpp
    PyImathAutovectorize.cpp
    PyImathBox2Array.cpp
    PyImathBox3Array.cpp
    PyImathBox.cpp
    PyImathColor3.cpp
    PyImathColor4.cpp
    PyImathEuler.cpp
    PyImathFixedArray.cpp
    PyImathFrustum.cpp
    PyImathLine.cpp
    PyImathMatrix33.cpp
    PyImathMatrix44.cpp
    PyImathPlane.cpp
    PyImathQuat.cpp
    PyImathRandom.cpp
    PyImathShear.cpp
    PyImathStringArray.cpp
    PyImathStringTable.cpp
    PyImathTask.cpp
    PyImathUtil.cpp
    PyImathFixedVArray.cpp
    PyImathVec2fd.cpp
    PyImathVec2si.cpp
    PyImathVec3fd.cpp
    PyImathVec3siArray.cpp
    PyImathVec3si.cpp
    PyImathVec4fd.cpp
    PyImathVec4siArray.cpp
    PyImathVec4si.cpp

)

TARGET_LINK_LIBRARIES ( PyImath
	Iex${ILMBASE_LIBSUFFIX}
	IexMath${ILMBASE_LIBSUFFIX}
	Imath${ILMBASE_LIBSUFFIX}
	${Boost_LIBRARIES}
	${PYTHON_LIBRARIES}
)

INSTALL ( TARGETS PyImath
	DESTINATION
	lib
)

set(IMATHMODULE_LIB_TYPE ${LIB_TYPE})

ADD_LIBRARY ( imathmodule  ${IMATHMODULE_LIB_TYPE}
    imathmodule.cpp
    PyImathFun.cpp
    PyImathBasicTypes.cpp 
)

# try to use static pyimath
#add_definitions(-DPLATFORM_BUILD_STATIC)
#set_target_properties(imathmodule PROPERTIES COMPILE_FLAGS -DPLATFORM_BUILD_STATIC)
#set_target_properties(imathmodule PROPERTIES COMPILE_FLAGS -DPYIEX_EXPORTS)
#set_target_properties(imathmodule PROPERTIES COMPILE_FLAGS -DPYIMATH_EXPORTS)

IF(WIN32)
    SET_TARGET_PROPERTIES ( imathmodule
    PROPERTIES PREFIX "" BUILD_WITH_INSTALL_RPATH ON
    OUTPUT_NAME "imath"
    SUFFIX ".pyd"
    )
ELSEIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    SET_TARGET_PROPERTIES ( imathmodule
    PROPERTIES PREFIX "" SUFFIX ".so" BUILD_WITH_INSTALL_RPATH ON
    )
ENDIF()

TARGET_LINK_LIBRARIES ( imathmodule
	PyImath
	PyIex
	Imath${ILMBASE_LIBSUFFIX}
	Iex${ILMBASE_LIBSUFFIX}
	${Boost_LIBRARIES}
)

INSTALL ( TARGETS imathmodule
	DESTINATION lib/python${PYTHON_VERSION_MAJOR}.${PYTHON_VERSION_MINOR}/site-packages
)

install(DIRECTORY ${PROJECT_INCLUDE_DIR}/
    DESTINATION "include/OpenEXR"
    FILES_MATCHING
    PATTERN "*.h"
    )