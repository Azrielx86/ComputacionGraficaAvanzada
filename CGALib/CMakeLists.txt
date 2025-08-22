add_definitions(-DBUILDING_DLL) # Define a macro indicating DLL import

include(FetchContent)

file(GLOB CGALib_SRC
        "./include/Headers/*.h"
        "./src/*.cpp"
)

if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    message(STATUS "Compiling Debug version.")
    add_compile_options(-O0 -gfull)
    add_link_options(-O0 -gfull)
else ()
    add_compile_options(-O3)
endif ()


if (VCPKG_TOOLCHAIN)
    message(STATUS "Using vcpkg Package Manager")

    set(OpenGL_GL_PREFERENCE GLVND)

    include_directories(
            CGALib
            "./include/"
    )

    find_package(assimp REQUIRED)
    find_package(glm CONFIG REQUIRED)
    find_package(OpenGL REQUIRED)
    find_package(GLEW REQUIRED)
    find_package(glfw3 CONFIG REQUIRED)
    find_package(glm CONFIG REQUIRED)
    find_package(freeimage CONFIG REQUIRED)
    find_package(Freetype REQUIRED)

    add_library(CGALib SHARED ${CGALib_SRC})

    target_link_libraries(CGALib PRIVATE OpenGL::GL)
    target_link_libraries(CGALib PRIVATE glfw)
    target_link_libraries(CGALib PRIVATE GLEW::GLEW)
    target_link_libraries(CGALib PRIVATE glm::glm)
    target_link_libraries(CGALib PRIVATE assimp::assimp)
    target_link_libraries(CGALib PRIVATE freeimage::FreeImage freeimage::FreeImagePlus)
    target_link_libraries(CGALib PRIVATE Freetype::Freetype)
else ()
    if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
        include_directories(
                CGALib
                "./include/"
                "../external/assimp/include"
                "../external/glfw/include"
                "../external/glm"
                "../external/glew/include"
                "../external/FreeImage/include"
                "../external/OpenAL/include"
                "../external/Freetype/include"
                "../external/freealut/include"

        )
        link_directories(
                "../external/assimp/lib"
                "../external/glfw/lib"
                "../external/Freetype/lib"
                "../external/FreeImage/lib"
                "../external/glew/lib"
        )
    elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
        include_directories(
                CGALib
                "./include/"
                "/usr/include/freetype2"
        )
    endif ()
    add_library(CGALib SHARED ${CGALib_SRC})
    if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
        set_target_properties(CGALib
                PROPERTIES
                RUNTIME_OUTPUT_DIRECTORY "../../bin"
        )
        target_link_libraries(CGALib opengl32.lib libglfw3dll.dll.a libglew32.dll.a libassimp.dll.a libfreetype.dll.a)
    elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
        target_link_libraries(CGALib glut glfw GLEW GLU GL freeimage assimp freetype)
    endif ()
endif ()