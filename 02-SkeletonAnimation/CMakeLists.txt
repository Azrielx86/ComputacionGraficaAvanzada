add_definitions(-DBUILDING_DLL) # Define a macro indicating DLL import

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

    include_directories(
            02-SkeletonAnimation
            ${CMAKE_SOURCE_DIR}/CGALib/include
    )

    add_executable(02-SkeletonAnimation ./src/main.cpp)

    set_target_properties(02-SkeletonAnimation
            PROPERTIES
            RUNTIME_OUTPUT_DIRECTORY "../../bin"
    )

    target_link_libraries(02-SkeletonAnimation PRIVATE CGALib)
    target_link_libraries(02-SkeletonAnimation PRIVATE OpenGL::GL)
    target_link_libraries(02-SkeletonAnimation PRIVATE glfw)
    target_link_libraries(02-SkeletonAnimation PRIVATE GLEW::GLEW)
    target_link_libraries(02-SkeletonAnimation PRIVATE glm::glm)
    target_link_libraries(02-SkeletonAnimation PRIVATE assimp::assimp)
    target_link_libraries(02-SkeletonAnimation PRIVATE freeimage::FreeImage freeimage::FreeImagePlus)
    target_link_libraries(02-SkeletonAnimation PRIVATE Freetype::Freetype)
else ()
    if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
        # Set the directory where DLLs are located
        set(CMAKE_DLLTOOLCHAIN_PREFIX "../dlls")
        include_directories(
                02-SkeletonAnimation
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

    add_executable(02-SkeletonAnimation ./src/main.cpp)

    set_target_properties(02-SkeletonAnimation
            PROPERTIES
            RUNTIME_OUTPUT_DIRECTORY "../../bin"
    )


    if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
        target_link_libraries(02-SkeletonAnimation opengl32.lib libglfw3dll.dll.a libglew32.dll.a libassimp.dll.a libfreetype.dll.a libalut.dll.a OpenAL32.lib CGALib)
    elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
        target_link_libraries(02-SkeletonAnimation glut glfw GLEW GLU GL freeimage assimp freetype alut openal CGALib)
    endif ()
endif ()
