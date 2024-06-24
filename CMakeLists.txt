cmake_minimum_required(VERSION 3.12)

project(TSOAX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON) # For meta object compiler
set(CMAKE_AUTORCC ON) # Resource files
set(CMAKE_AUTOUIC ON) # UI files

# A preferred way to setup Qt5 dependencies. See more at
# https://blog.kitware.com/cmake-finding-qt5-the-right-way/. Before configure,
# Qt5_DIR should be set by 'export Qt5_DIR="/usr/local/opt/qt5"' in your
# .bashrc. Set CMAKE_PREFIX_PATH is discouraged.
find_package(Qt6 COMPONENTS Widgets REQUIRED)
find_package(VTK REQUIRED)
find_package(OpenMP REQUIRED)
include(${VTK_USE_FILE})

add_definitions(-DBOOST_ERROR_CODE_HEADER_ONLY)

# # Enable these options for Windows build
# if (WIN32)
set(Boost_USE_STATIC_LIBS ON)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)

# endif()
find_package(Boost COMPONENTS
  program_options filesystem system REQUIRED)

if(Boost_FOUND)
  include_directories(${Boost_INCLUDE_DIRS})
endif(Boost_FOUND)

set(SRC_SHARED
  include/actor_color.h
  src/actor_color.cc
  include/gradient_calculator.h
  src/gradient_calculator.cc
  include/grid.h
  src/grid.cc
  include/image.h
  src/image.cc
  include/image_plane.h
  src/image_plane.cc
  include/image_reader.h
  src/image_reader.cc
  include/image_resampler.h
  src/image_resampler.cc
  include/interpolator.h
  src/interpolator.cc
  include/junction_actor.h
  src/junction_actor.cc
  include/junctions.h
  src/junctions.cc
  include/lapjv.h
  src/lapjv.cc
  include/multisnake.h
  src/multisnake.cc
  include/munkres.h
  src/munkres.cc
  include/slice_planes.h
  src/slice_planes.cc
  include/snake.h
  src/snake.cc
  include/snake_actor.h
  src/snake_actor.cc
  include/snake_parameters.h
  src/snake_parameters.cc
  include/snake_tip.h
  src/snake_tip.cc
  include/snake_tip_set.h
  src/snake_tip_set.cc
  include/snake_track.h
  src/snake_track.cc
  include/util.h
  src/util.cc)

SET(SRC_GUI
  src/analysis_options_dialog.cc
  src/view_options_dialog.cc
  src/main_window.cc
  src/parameters_dialog.cc
  src/viewer.cc
  src/viewpoint.cc
  src/volume_rendering.cc)

if(APPLE)
  # set(ICON_FOLDER icon)
  # set(ICON_NAME troax.icns)
  # set(ICON_FILE ${ICON_FOLDER}/${ICON_NAME})
  # set_source_files_properties(${ICON_FILE} PROPERTIES
  #   MACOSX_PACKAGE_LOCATION Resources)

  # set(COPYRIGHT "Copyright (c) 2018 Lehigh University. All rights reserved.")
  # set(IDENTIFIER "edu.lehigh.tsoax")

  # set(PROJECT_VERSION 0.2.0)
  # set(MACOSX_BUNDLE_BUNDLE_NAME ${PROJECT_NAME})
  # set(MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION})
  # set(MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION})
  # set(MACOSX_BUNDLE_COPYRIGHT ${COPYRIGHT})
  # set(MACOSX_BUNDLE_GUI_IDENTIFIER ${IDENTIFIER})
  # set(MACOSX_BUNDLE_ICON_FILE ${ICON_NAME})
  # add_executable(tsoax_gui MACOSX_BUNDLE ${SRC_SHARED} ${SRC_GUI} src/icons.qrc
  #   ${ICON_FILE})
  # set_target_properties(tsoax_gui PROPERTIES MACOSX_BUNDLE_INFO_PLIST
  #   ${CMAKE_SOURCE_DIR}/TroaxBundleInfo.plist.in)
  # target_link_libraries(tsoax_gui Qt5::Widgets ${VTK_LIBRARIES} ${Boost_FILESYSTEM_LIBRARY} ${Boost_SYSTEM_LIBRARY} OpenMP::OpenMP_CXX)

else(APPLE)
  add_executable(tsoax_gui ${SRC_SHARED} ${SRC_GUI} src/troax.cc src/icons.qrc)
  target_link_libraries(tsoax_gui Qt6::Widgets ${VTK_LIBRARIES} ${Boost_FILESYSTEM_LIBRARY} ${Boost_SYSTEM_LIBRARY} OpenMP::OpenMP_CXX)
endif(APPLE)

add_executable(tsoax ${SRC_SHARED} src/batch_tsoax.cc)
target_link_libraries(tsoax Qt6::Core ${VTK_LIBRARIES} ${Boost_PROGRAM_OPTIONS_LIBRARY} ${Boost_FILESYSTEM_LIBRARY} ${Boost_SYSTEM_LIBRARY} OpenMP::OpenMP_CXX)
