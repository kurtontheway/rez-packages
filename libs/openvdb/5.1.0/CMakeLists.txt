CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

include(RezBuild)
include(RezRepository)
include(ExternalProject)
include(ProcessorCount)
ProcessorCount(ncpus)

set(version $ENV{REZ_BUILD_PROJECT_VERSION})
set(name $ENV{REZ_BUILD_PROJECT_NAME})
set(url https://github.com/dreamworksanimation/${name}/archive/v${version}.tar.gz)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

if(${REZ_BUILD_INSTALL})
    set(install_cmd make install)
else()
    set(install_cmd "")
endif()


ExternalProject_add(
    ${name}
    URL ${url}
    PREFIX ${name}
    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
    -DBLOSC_LOCATION=$ENV{REZ_BLOSC_ROOT}
    -DTBB_LOCATION=$ENV{REZ_TBB_ROOT}
    -DILMBASE_LOCATION=$ENV{REZ_OPENEXR_ROOT}
    -DOPENEXR_LOCATION=$ENV{REZ_OPENEXR_ROOT}
    -DIlmbase_IEX_LIBRARY=$ENV{REZ_OPENEXR_ROOT}/lib/libIex.so
    -DIlmbase_ILMTHREAD_LIBRARY=$ENV{REZ_OPENEXR_ROOT}/lib/libIlmThread.so
    -DOpenexr_ILMIMF_LIBRARY=$ENV{REZ_OPENEXR_ROOT}/lib/libIlmImf.so
    -DUSE_GLFW3=ON
    -DGLFW3_LOCATION=$ENV{REZ_GLFW_ROOT}
    # -DCPPUNIT_LOCATION=$ENV{REZ_CPPUNIT_ROOT}
    -DCMAKE_CXX_FLAGS=-std=c++11
    -DOPENVDB_BUILD_UNITTESTS=OFF
    -DOPENVDB_ABI_VERSION_NUMBER=3
    INSTALL_COMMAND "${install_cmd}"
)

