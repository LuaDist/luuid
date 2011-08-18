# Copyright (C) 2011 LuaDist.
# Created by Peter Kapec 
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(luuid C)
CMAKE_MINIMUM_REQUIRED (VERSION 2.6)
INCLUDE( dist.cmake )

# Find Lua
FIND_PACKAGE (Lua51 REQUIRED)
INCLUDE_DIRECTORIES( ${LUA_INCLUDE_DIR})

# Find uuid lib
IF(UNIX)
	FIND_PACKAGE(UUID REQUIRED)
	SET(LIBS ${UUID_LIBRARIES})
ENDIF()

# Set sources
IF(WIN32 AND NOT CYGWIN)
	SET(SRC wuuid.c)
	SET(LIBS rpcrt4)
ELSE(WIN32 AND NOT CYGWIN)
	SET(SRC luuid.c)
ENDIF(WIN32 AND NOT CYGWIN)

# Build
ADD_LIBRARY(uuid MODULE luuid.c ${SRC})
TARGET_LINK_LIBRARIES(uuid ${LUA_LIBRARY} ${LIBS})
SET_TARGET_PROPERTIES(uuid PROPERTIES PREFIX "")

IF(MSVC)
	SET_TARGET_PROPERTIES(uuid PROPERTIES LINK_FLAGS "/DEF:${CMAKE_CURRENT_SOURCE_DIR}/uuid.def")
ENDIF(MSVC)

# Install all files and documentation
INSTALL (TARGETS uuid DESTINATION ${INSTALL_CMOD})
INSTALL (FILES test.lua DESTINATION ${INSTALL_TEST})
INSTALL (FILES README README.win32 DESTINATION ${INSTALL_DATA})
