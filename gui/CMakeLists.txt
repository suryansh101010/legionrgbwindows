cmake_minimum_required(VERSION 3.16)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

find_package(Qt6 6.5 REQUIRED COMPONENTS Widgets Multimedia)
find_package(Threads REQUIRED)
include_directories(
    ${CMAKE_SOURCE_DIR}/lib
    ${CMAKE_CURRENT_SOURCE_DIR}
)

add_executable(legionaura-gui
    main.cpp
    MainWindow.cpp
    ScreenSync.cpp
    ScreenSync.h
    ScreenColors.h
    MainWindow.h
    MainWindow.ui
    KeyboardPreview.cpp
    KeyboardPreview.h
    accent/AccentProvider.h
    accent/AccentManager.h
    accent/AccentManager.cpp
)

target_link_libraries(legionaura-gui
    Qt6::Widgets
    Qt6::Multimedia
    Threads::Threads
    legionaura_lib
)

install(TARGETS legionaura-gui
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
