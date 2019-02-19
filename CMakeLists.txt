cmake_minimum_required(VERSION 2.8.7)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")
set(PROJECT
    server.cpp)

add_definitions(-std=c++11)


set(Boost_USE_STATIC_LIBS OFF) 
set(Boost_USE_MULTITHREADED ON)  
set(Boost_USE_STATIC_RUNTIME OFF) 
set(Boost_NO_SYSTEM_PATHS TRUE) 

if (Boost_NO_SYSTEM_PATHS)
  set(BOOST_ROOT "/usr/local/boost_1_69_0")
  set(BOOST_INCLUDE_DIRS "usr/include/boost")
  set(BOOST_LIBRARY_DIRS "/usr/local/boost_1_69_0/stage/lib")
endif (Boost_NO_SYSTEM_PATHS)


set (OPENCV_ROOT "/usr/share/opencv")
set (OPENCV_INCLUDE_DIRS "${OPENCV_ROOT}/include")
set (OPENCV_LIBRARY_DIRS "${OPENCV_ROOT}/lib")


find_package(OpenCV)
find_package(OpenCV COMPONENTS core imgproc highgui)
find_package(OpenCV REQUIRED core imgproc highgui)
find_package(OpenCV COMPONENTS core imgproc highgui REQUIRED)

#find_package(Boost COMPONENTS regex date_time system filesystem thread graph program_options) 
#find_package(Boost REQUIRED regex date_time system filesystem thread graph program_options)
#find_package(Boost COMPONENTS program_options REQUIRED)


foreach(PROJECT_FILE ${PROJECT})

  get_filename_component(SOURCE_NAME ${PROJECT_FILE} NAME_WE)

  include_directories(${OpenCV_INCLUDE_DIRS} )

  set(EXE_NAME "${SOURCE_NAME}.bin")

  message(STATUS "Adding Example ${EXE_NAME}")
  add_executable(${EXE_NAME} ${PROJECT_FILE} )

  include_directories( ${OpenCV_INCLUDE_DIRS} )
  link_directories(${OPENCV_LIBRARY_DIRS})
  target_link_libraries(server.bin ${OpenCV_LIBRARY_DIRS} ${OpenCV_LIBS})

  target_link_libraries(server.bin ${OpenCV_CORE_LIBRARY} ${OpenCV_IMGPROC_LIBRARY} ${OpenCV_HIGHGUI_LIBRARY} ${OpenCV_LIBS})


if(Boost_FOUND)
    include_directories(${BOOST_INCLUDE_DIRS})
    link_directories(${BOOST_LIBRARY_DIRS})
    add_definitions(${BOOST_DEFINITIONS})

    include_directories(${BOOST_INCLUDE_DIRS})  
    target_link_libraries(server.bin ${BOOST_LIBRARY_DIRS} ${OpenCV_LIBS})
    target_link_libraries( server.bin ${BOOST_LIBRARY_DIRS}/libboost_program_options.a ${BOOST_LIBRARY_DIRS}/libboost_filesystem.a ${BOOST_LIBRARY_DIRS}/libboost_regex.a ${BOOST_LIBRARY_DIRS}/libboost_system.a ${BOOST_LIBRARY_DIRS}/libboost_thread.a ${OpenCV_LIBS})
endif()

endforeach()
