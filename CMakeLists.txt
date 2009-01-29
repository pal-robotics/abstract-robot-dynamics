# Olivier Stasse, JRL, CNRS/AIST
# Creation: 31/10/2008
# History:
#
# Copyright CNRS/AIST

cmake_minimum_required(VERSION 2.6)

enable_language(CXX)


# Debug mode for cmake.
SET(CMAKE_VERBOSE_MAKEFILE ON)

# Set version
SET(PROJECT_VERSION 1.11.100)


# name of the project
SET(PROJECT_NAME abstractRobotDynamics)
SET(PROJECT_CONTACT openrobots@laas)

project(${PROJECT_NAME})

SET(CMAKE_INSTALL_PREFIX $ENV{ROBOTPKG_PATH})

# Check for required packages
# -------------------------
INCLUDE(FindPkgConfig)
pkg_check_modules(MALJRL REQUIRED MatrixAbstractLayer>=1.5.99 )

SET(PROJECT_REQUIREMENTS "MatrixAbstractLayer >= 1.5.99")
			  
# To deal with the headers.
#--------------------------
add_subdirectory(interfaces)

# Prepare the documentation
# -------------------------
INCLUDE(FindDoxygen)
IF(DOXYGEN_FOUND)
  MESSAGE(STATUS "Doxygen found")
  add_subdirectory(documentation)
ENDIF(DOXYGEN_FOUND)

# Prepare description file for pkg-config.
#-----------------------------------------

SET(install_pkg_prefix "\${prefix}")
SET(install_pkg_exec_prefix "\${exec_prefix}")
SET(install_pkg_libdir "\${libdir}")
SET(install_pkg_include_dir "\${includedir}")

# Install the file in the appropriate place.
SET(install_libdir_pkgconfig ${CMAKE_INSTALL_PREFIX}/lib/pkgconfig)

CONFIGURE_FILE(${${PROJECT_NAME}_SOURCE_DIR}/${PROJECT_NAME}.pc.cmake
		 ${${PROJECT_NAME}_BINARY_DIR}/${PROJECT_NAME}.pc )

INSTALL(FILES ${${PROJECT_NAME}_BINARY_DIR}/${PROJECT_NAME}.pc
	DESTINATION ${install_libdir_pkgconfig}	
        PERMISSIONS OWNER_READ GROUP_READ WORLD_READ OWNER_WRITE
)




