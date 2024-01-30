cmake_minimum_required(VERSION 3.16)
project(SFMLTemplate LANGUAGES CXX)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)

set(CPACK_PACKAGE_NAME "SFMLTemplate")
set(CPACK_PACKAGE_VERSION "YourVersionHere")
set(CPACK_PACKAGE_DIRECTORY "${PROJECT_SOURCE_DIR}/deploy")

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

include(FetchContent)
FetchContent_Declare(SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG 2.6.x)
FetchContent_MakeAvailable(SFML)

add_executable(SFMLTemplate src/main.cpp)
target_link_libraries(SFMLTemplate PRIVATE sfml-graphics)
target_compile_features(SFMLTemplate PRIVATE cxx_std_17)

if(WIN32)
    add_custom_command(
        TARGET SFMLTemplate
        COMMENT "Copy OpenAL DLL"
        PRE_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${SFML_SOURCE_DIR}/extlibs/bin/$<IF:$<EQUAL:${CMAKE_SIZEOF_VOID_P},8>,x64,x86>/openal32.dll $<TARGET_FILE_DIR:SFMLTemplate>
        VERBATIM)
endif()

install(TARGETS SFMLTemplate)
