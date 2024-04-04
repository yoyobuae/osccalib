# CMakeLists.txt
cmake_minimum_required(VERSION 3.0)
project(osccalib VERSION 0.0.1)

# Find dependencies provided by vcpkg (via vcpkg.cmake)
find_package(imgui CONFIG REQUIRED)
find_package(OpenGL REQUIRED)

# main target
add_executable(osccalib)
target_sources(osccalib PRIVATE main.cpp osc/OscOutboundPacketStream.cpp osc/OscPrintReceivedElements.cpp osc/OscReceivedElements.cpp osc/OscTypes.cpp ip/IpEndpointName.cpp)
if(CMAKE_SYSTEM_NAME MATCHES "Linux")
  message(DEBUG "Linux")
  target_sources(osccalib PRIVATE ip/posix/NetworkingUtils.cpp ip/posix/UdpSocket.cpp)
elseif(CMAKE_SYSTEM_NAME MATCHES "Windows")
  message(DEBUG "Windows")
  target_sources(osccalib PRIVATE ip/win32/NetworkingUtils.cpp ip/win32/UdpSocket.cpp)
  target_link_libraries(osccalib PRIVATE wsock32 ws2_32 winmm)
else()
  message(FATAL "No OS detected!")
endif(CMAKE_SYSTEM_NAME MATCHES "Linux")

target_link_libraries(osccalib PRIVATE imgui::imgui)
target_link_libraries(osccalib PRIVATE OpenGL::GL)
target_include_directories(osccalib PRIVATE .)
set_property(TARGET osccalib PROPERTY CXX_STANDARD 20)

install(TARGETS osccalib)
install(FILES "imgui.ini" TYPE BIN)
if(CMAKE_SYSTEM_NAME MATCHES "Windows")
	install(FILES "${CMAKE_BINARY_DIR}/Release/SDL2.dll" TYPE BIN)
endif(CMAKE_SYSTEM_NAME MATCHES "Windows")

include(CPack)
