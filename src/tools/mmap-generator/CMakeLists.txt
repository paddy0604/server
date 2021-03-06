# mangos-zero is a full featured server for World of Warcraft in its vanilla
# version, supporting clients for patch 1.12.x.
#
# Copyright (C) 2005-2013  MaNGOS project <http://getmangos.com>
#
# ***** BEGIN GPL LICENSE BLOCK *****
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
#
# ***** END GPL LICENSE BLOCK *****
#
# World of Warcraft, and all World of Warcraft or Warcraft art, images,
# and lore are copyrighted by Blizzard Entertainment, Inc.

include(MacroMangosSourceGroup)

#-----------------------------------------------------------------------------
# Define the mmap-generator application
file(GLOB sources *.cpp)
file(GLOB headers *.h)

set(mmap_generator_LIB_SRCS ${sources} ${headers})

mangos_source_group(${mmap_generator_LIB_SRCS})

include_directories(
    ${ACE_INCLUDE_DIRS}
    ${MYSQL_INCLUDE_DIR}
    ${ZLIB_INCLUDE_DIR}
    ${CMAKE_SOURCE_DIR}/dep/include/g3dlite/
    ${CMAKE_SOURCE_DIR}/dep/recastnavigation/Detour/Include
    ${CMAKE_SOURCE_DIR}/dep/recastnavigation/Recast/Include
    ${CMAKE_SOURCE_DIR}/src/framework/
    ${CMAKE_SOURCE_DIR}/src/shared/
    ${CMAKE_SOURCE_DIR}/src/game/
    ${CMAKE_SOURCE_DIR}/src/game/vmap/
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_BINARY_DIR}
    ${CMAKE_BINARY_DIR}/src/shared/
)

#-----------------------------------------------------------------------------
# Build the mmap-generator application
add_executable(mmap-generator ${mmap_generator_LIB_SRCS})
target_link_libraries(mmap-generator mangos-vmap mangos-shared mangos-framework g3dlite Detour Recast ${ACE_LIBRARIES} ${ZLIB_LIBRARIES})

set(BUILD_PROPERTIES "-DMMAP_GENERATOR -DNO_CORE_FUNCS")
if(WIN32)
    # Disabled for now. GnuWin32-zlib does not need this
    # set(BUILD_PROPERTIES "${BUILD_PROPERTIES} -DZLIB_WINAPI" )
endif()
set_target_properties(mmap-generator PROPERTIES COMPILE_FLAGS ${BUILD_PROPERTIES})
install(TARGETS mmap-generator DESTINATION ${BIN_DIR})
