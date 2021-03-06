#
#   This file is part of BlueFramework, a simple 3D engine.
#	Copyright (c) 2019 Technical University of Munich
#	Chair of Computational Modeling and Simulation.
#   
#   BlueFramework is free software; you can redistribute it and/or modify
#   it under the terms of the GNU General Public License Version 3
#   as published by the Free Software Foundation.
#   
#   BlueFramework is distributed in the hope that it will be useful,
#   but WITHOUT ANY WARRANTY; without even the implied warranty of
#   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
#   GNU General Public License for more details.
#   
#   You should have received a copy of the GNU General Public License
#   along with this program. If not, see <http://www.gnu.org/licenses/>.
#
find_package(OpenGL REQUIRED)

FetchContent_Declare(
	glew
	URL https://sourceforge.net/projects/glew/files/glew/2.0.0/glew-2.0.0.zip/download
)
  
if(NOT glew_POPULATED)
	FetchContent_Populate(glew)
	add_subdirectory(${glew_SOURCE_DIR}/build/cmake ${glew_SOURCE_DIR}/build EXCLUDE_FROM_ALL)
endif()

get_target_property(glew_INCLUDE_DIR glew_s INCLUDE_DIRECTORIES)

file(GLOB_RECURSE
	BlueFramework_GLRenderSystem_Source
	LIST_DIRECTORIES false
	src/BlueFramework/GLRenderSystem/*.*
)

file(GLOB_RECURSE
	BlueFramework_GLRenderSystem_Headers
	LIST_DIRECTORIES false
	src/BlueFramework/GLRenderSystem/*.h
)

source_group(BlueFramework\\GLRenderSystem
	FILES ${BlueFramework_GLRenderSystem_Source}
)

add_library(BlueFramework.GLRenderSystem
	SHARED
	${BlueFramework_GLRenderSystem_Source}
)

target_include_directories(BlueFramework.GLRenderSystem
	PUBLIC
		src
		${glew_INCLUDE_DIR}
)

target_link_libraries(BlueFramework.GLRenderSystem
	PUBLIC
		BlueFramework.Core
		BlueFramework.Rasterizer
		glew_s
		tinyxml2
	PRIVATE
		${OPENGL_gl_LIBRARY}
)

install(TARGETS BlueFramework.GLRenderSystem   
   ARCHIVE DESTINATION lib/${MSVC_VERSION_STRING}/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>
   COMPONENT library_files
)

install(DIRECTORY src/BlueFramework/GLRenderSystem/ DESTINATION include/BlueFramework/GLRenderSystem
		COMPONENT include
        FILES_MATCHING PATTERN "*.h"
)

install(DIRECTORY src/BlueFramework/GLRenderSystem/ DESTINATION src/BlueFramework/GLRenderSystem
		COMPONENT source
        FILES_MATCHING PATTERN "*.cpp" PATTERN "*.h"
)
