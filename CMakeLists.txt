# Copyright (C) 2011-2012 LuaDist.
# Created by Peter Kapec 
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project ( luuid C )
cmake_minimum_required ( VERSION 2.6 )
include ( cmake/dist.cmake )
include ( lua )

# Find uuid lib
if ( UNIX )
	find_package ( UUID REQUIRED )
	set ( LIBS ${UUID_LIBRARIES} )
endif ()

if ( WIN32 AND NOT CYGWIN )
	set ( SRC wuuid.c )
	set ( LIBS rpcrt4 )
else ()
	set ( SRC luuid.c )
endif ()

install_lua_module ( uuid luuid.c ${SRC} uuid.def LINK ${LIBS} )
install_test ( test.lua )
install_data ( README README.win32 )

add_lua_test ( test.lua )
