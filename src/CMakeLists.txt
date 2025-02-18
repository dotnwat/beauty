add_library(beauty
    ../include/beauty/acceptor.hpp
    ../include/beauty/acceptor.hpp
    ../include/beauty/application.hpp
    ../include/beauty/attributes.hpp
    ../include/beauty/beauty.hpp
    ../include/beauty/certificate.hpp
    ../include/beauty/client.hpp
    ../include/beauty/exception.hpp
    ../include/beauty/header.hpp
    ../include/beauty/request.hpp
    ../include/beauty/response.hpp
    ../include/beauty/route.hpp
    ../include/beauty/router.hpp
    ../include/beauty/server.hpp
    ../include/beauty/session.hpp
    ../include/beauty/signal.hpp
    ../include/beauty/swagger.hpp
    ../include/beauty/timer.hpp
    ../include/beauty/url.hpp
    ../include/beauty/utils.hpp
    ../include/beauty/websocket_context.hpp
    ../include/beauty/websocket_session.hpp
    ./acceptor.cpp
    ./application.cpp
    ./attributes.cpp
    ./client.cpp
    ./exception.cpp
    ./route.cpp
    ./router.cpp
    ./server.cpp
    ./session_client.hpp
    ./signal.cpp
    ./swagger.cpp
    ./timer.cpp
    ./url.cpp
    ./utils.cpp
)

add_library(beauty::beauty ALIAS beauty)

set_target_properties(beauty
    PROPERTIES
        POSITION_INDEPENDENT_CODE ON
        RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
        LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
        ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
        OUTPUT_NAME beauty
        SOVERSION ${VERSION}
        VERSION ${VERSION}
)

if(BEAUTY_ENABLE_OPENSSL)
    target_compile_definitions(beauty
        PUBLIC
            BEAUTY_ENABLE_OPENSSL=1
    )
endif()

if(UNIX)
    target_compile_definitions(beauty
        PRIVATE
            LINUX
            UNIX
            _FORTIFY_SOURCE=2
    )
    set_target_properties(beauty
        PROPERTIES
            LINK_FLAGS "-Wl,-z,relro -Wl,--as-needed"
    )
elseif(WIN32)
    target_compile_definitions(beauty
        PUBLIC
            _WIN32_WINNT=0xA00
            _CRT_SECURE_NO_WARNINGS
            _ITERATOR_DEBUG_LEVEL=$<IF:$<CONFIG:Debug>,2,0>
    )
    target_compile_options(beauty PRIVATE "/bigobj")
endif()

target_include_directories(beauty
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/src>
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

target_link_libraries(beauty
    PUBLIC
        Boost::boost
)
if (BEAUTY_ENABLE_OPENSSL)
    target_link_libraries(beauty
        PUBLIC
            openssl::openssl
            ${CMAKE_DL_LIBS}
    )
endif()


if(UNIX)
    target_link_libraries(beauty
        PUBLIC
            Threads::Threads
            stdc++fs
    )
elseif(WIN32)
    target_link_libraries(beauty
        PUBLIC
            CRYPT32.LIB
    )
endif()

configure_file(./version.hpp.in ./beauty/version.hpp)

install(FILES "${CMAKE_BINARY_DIR}/src/beauty/version.hpp"
        DESTINATION include/beauty
)
install(
    DIRECTORY ${PROJECT_SOURCE_DIR}/include/
    DESTINATION include
)

install(TARGETS beauty
    EXPORT BeautyConfig
    ARCHIVE  DESTINATION lib
    LIBRARY  DESTINATION lib
)

install(EXPORT BeautyConfig
    FILE BeautyConfig.cmake
    NAMESPACE beauty::
    DESTINATION lib/cmake/beauty
)
