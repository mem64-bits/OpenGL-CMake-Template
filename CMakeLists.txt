cmake_minimum_required(VERSION 3.18)
project(OpenGLProject CXX)

if (MSVC)
    set(CMAKE_CXX_FLAGS_INIT "")
    set(CMAKE_CXX_FLAGS_DEBUG_INIT "/Od /RTC1")
    set(CMAKE_CXX_FLAGS_RELEASE_INIT "")
else ()
    set(CMAKE_CXX_FLAGS_INIT "")
    set(CMAKE_CXX_FLAGS_DEBUG_INIT "-g")
    set(CMAKE_CXX_FLAGS_RELEASE_INIT "")
endif ()

# Set the C++ standard
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(glfw3 CONFIG REQUIRED)
find_package(glad CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)
find_package(Stb REQUIRED)

file(GLOB SOURCES "src/*.cpp")
add_executable(${PROJECT_NAME} ${SOURCES})


file(GLOB_RECURSE ASSET_FILES CONFIGURE_DEPENDS "${CMAKE_SOURCE_DIR}/assets/*")
set(COPIED_ASSETS "")

foreach (ASSET_SOURCE_PATH ${ASSET_FILES})
    file(RELATIVE_PATH ASSET_REL_PATH "${CMAKE_SOURCE_DIR}" "${ASSET_SOURCE_PATH}")
    set(ASSET_DEST_PATH "${CMAKE_BINARY_DIR}/${ASSET_REL_PATH}")

    add_custom_command(
            OUTPUT ${ASSET_DEST_PATH}
            COMMAND ${CMAKE_COMMAND} -E copy_if_different ${ASSET_SOURCE_PATH} ${ASSET_DEST_PATH}
            DEPENDS ${ASSET_SOURCE_PATH}
            COMMENT "Copying asset: ${ASSET_REL_PATH}"
    )

    list(APPEND COPIED_ASSETS ${ASSET_DEST_PATH})
endforeach ()

add_custom_target(copy_assets ALL
        DEPENDS ${COPIED_ASSETS}
        COMMENT "Ensuring all project assets are up-to-date"
)

add_dependencies(${PROJECT_NAME} copy_assets)

if (MSVC)
    message(STATUS "Configuring for MSVC compiler")
    set(COMMON_COMPILER_FLAGS /W4 /external:W3 /permissive- /GS /MP)
    set(DEBUG_COMPILER_FLAGS /MDd)
    set(RELEASE_COMPILER_FLAGS /MD /O2 /Oi /Gy /arch:AVX2)

elseif (CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    message(STATUS "Configuring for GCC compiler")
    set(COMMON_COMPILER_FLAGS -Wall -Wextra -Wpedantic -fstack-protector-strong)
    set(DEBUG_COMPILER_FLAGS "") # '-g' is now handled by CMAKE_CXX_FLAGS_DEBUG_INIT
    set(RELEASE_COMPILER_FLAGS -O2 -march=native)

else ()
    message(WARNING "Unsupported compiler: ${CMAKE_CXX_COMPILER_ID}. No specific flags will be set.")
    set(COMMON_COMPILER_FLAGS "")
    set(DEBUG_COMPILER_FLAGS "")
    set(RELEASE_COMPILER_FLAGS "")
endif ()

target_include_directories(${PROJECT_NAME} PRIVATE ${Stb_INCLUDE_DIR})

target_compile_options(${PROJECT_NAME} PRIVATE
        ${COMMON_COMPILER_FLAGS}
        $<$<CONFIG:Debug>:${DEBUG_COMPILER_FLAGS}>
        $<$<CONFIG:Release>:${RELEASE_COMPILER_FLAGS}>
)

target_compile_definitions(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Release>:NDEBUG>
)

set_property(TARGET ${PROJECT_NAME} PROPERTY INTERPROCEDURAL_OPTIMIZATION_RELEASE TRUE)

target_link_libraries(${PROJECT_NAME} PRIVATE
        glad::glad
        glfw
        glm::glm
)