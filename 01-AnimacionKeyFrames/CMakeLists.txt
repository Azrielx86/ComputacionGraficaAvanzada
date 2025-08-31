add_definitions(-DBUILDING_DLL) # Define a macro indicating DLL import

if (VCPKG_TOOLCHAIN)
    set(OpenGL_GL_PREFERENCE GLVND)

    include_directories(
            CGALib
            ${CMAKE_SOURCE_DIR}/CGALib/include
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
elseif (CMAKE_SYSTEM_NAME STREQUAL "Windows")
    # Set the directory where DLLs are located
    set(CMAKE_DLLTOOLCHAIN_PREFIX "../dlls")
    include_directories(
            01-AnimacionKeyFrames
            ${CMAKE_SOURCE_DIR}/CGALib/include
            "/usr/include/freetype2"
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
            "../external/freealut/lib"
            "../external/OpenAL/lib/x64"
    )
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
    include_directories(${CMAKE_SOURCE_DIR}/CGALib/include)
endif ()

add_executable(01-AnimacionKeyFrames ./src/main.cpp)

set_target_properties(01-AnimacionKeyFrames
        PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY "../../bin"
)

if (VCPKG_TOOLCHAIN)
    target_link_libraries(01-AnimacionKeyFrames PRIVATE OpenGL::GL)
    target_link_libraries(01-AnimacionKeyFrames PRIVATE glfw)
    target_link_libraries(01-AnimacionKeyFrames PRIVATE GLEW::GLEW)
    target_link_libraries(01-AnimacionKeyFrames PRIVATE glm::glm)
    target_link_libraries(01-AnimacionKeyFrames PRIVATE assimp::assimp)
    target_link_libraries(01-AnimacionKeyFrames PRIVATE freeimage::FreeImage freeimage::FreeImagePlus)
    target_link_libraries(01-AnimacionKeyFrames PRIVATE Freetype::Freetype)
    target_link_libraries(01-AnimacionKeyFrames PRIVATE CGALib)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Windows")
    target_link_libraries(01-AnimacionKeyFrames opengl32.lib libglfw3dll.dll.a libglew32.dll.a libassimp.dll.a libfreetype.dll.a libalut.dll.a OpenAL32.lib CGALib)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
    target_link_libraries(01-AnimacionKeyFrames glut glfw GLEW GLU GL freeimage assimp freetype alut openal CGALib)
endif ()