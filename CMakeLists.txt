cmake_minimum_required(VERSION 3.16)
project(LegionAura VERSION 2.2.0)

include(GNUInstallDirs)

add_definitions(-DLEGIONAURA_VERSION=\"${PROJECT_VERSION}\")
add_compile_definitions(PROJECT_SOURCE_DIR="${CMAKE_SOURCE_DIR}")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Subdirs
add_subdirectory(lib)
add_subdirectory(cli)
add_subdirectory(gui)

# Linux desktop integration is not applicable to Windows.
if(NOT WIN32)
    install(FILES devices/devices.json DESTINATION ${CMAKE_INSTALL_DATADIR}/legionaura)
    install(FILES udev/60-legionaura.rules DESTINATION lib/udev/rules.d)
    install(FILES legionaura.desktop DESTINATION ${CMAKE_INSTALL_DATADIR}/applications)
    install(FILES icons/legionaura.png DESTINATION ${CMAKE_INSTALL_DATADIR}/icons/hicolor/256x256/apps)
    install(FILES legionaura-autostart.desktop DESTINATION ${CMAKE_INSTALL_DATADIR}/xdg/autostart)
endif()

include(CTest)
if(BUILD_TESTING)
    find_package(Qt6 6.5 REQUIRED COMPONENTS Gui)
    add_executable(screen-colors-test tests/ScreenColorsTest.cpp)
    target_include_directories(screen-colors-test PRIVATE gui)
    target_link_libraries(screen-colors-test PRIVATE Qt6::Gui)
    add_test(NAME screen-colors COMMAND screen-colors-test)

    add_executable(accent-color-test
        tests/AccentColorTest.cpp
        gui/accent/AccentManager.cpp
    )
    target_include_directories(accent-color-test PRIVATE gui)
    target_link_libraries(accent-color-test PRIVATE Qt6::Gui)
    add_test(NAME accent-color COMMAND accent-color-test)
endif()
