if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    add_definitions(-DUSE_SCHANNEL -DUSE_WINDOWS_SSPI)
else()
    add_definitions(-DHAVE_CONFIG_H)
endif()

file(GLOB SRCS *.c vauth/*.c vtls/*.c)
add_library(
  curl
  STATIC
  ${SRCS}
  )
dolphin_disable_warnings_msvc(curl)

target_include_directories(curl PRIVATE . INTERFACE ../include)
target_link_libraries(curl MbedTLS::mbedtls zlibstatic)
target_compile_definitions(curl PUBLIC CURL_STATICLIB PRIVATE CURL_DISABLE_LDAP)
add_library(CURL::libcurl ALIAS curl)
