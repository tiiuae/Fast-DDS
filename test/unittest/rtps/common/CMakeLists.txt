# Copyright 2016 Proyectos y Sistemas de Mantenimiento SL (eProsima).
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

set(CACHECHANGETESTS_SOURCE CacheChangeTests.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/rtps/common/Time_t.cpp)
set(GUID_UTILS_TESTS_SOURCE GuidUtilsTests.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/fastdds/log/Log.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/fastdds/log/OStreamConsumer.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/fastdds/log/StdoutConsumer.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/fastdds/log/StdoutErrConsumer.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/utils/IPFinder.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/utils/IPLocator.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/utils/md5.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/utils/SystemInfo.cpp)

if(ANDROID)
    if (ANDROID_NATIVE_API_LEVEL LESS 24)
        list(APPEND GUID_UTILS_TESTS_SOURCE
            ${ANDROID_IFADDRS_SOURCE_DIR}/ifaddrs.c
            )
        list(APPEND CACHECHANGETESTS_SOURCE
            ${ANDROID_IFADDRS_SOURCE_DIR}/ifaddrs.c
            )
        
    endif()
endif()


set(SEQUENCENUMBERTESTS_SOURCE SequenceNumberTests.cpp)
set(PORTPARAMETERSTESTS_SOURCE PortParametersTests.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/fastdds/log/Log.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/fastdds/log/OStreamConsumer.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/fastdds/log/StdoutConsumer.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/fastdds/log/StdoutErrConsumer.cpp
    ${PROJECT_SOURCE_DIR}/src/cpp/utils/SystemInfo.cpp)

add_executable(CacheChangeTests ${CACHECHANGETESTS_SOURCE})
target_compile_definitions(CacheChangeTests PRIVATE FASTRTPS_NO_LIB
    $<$<AND:$<NOT:$<BOOL:${WIN32}>>,$<STREQUAL:"${CMAKE_BUILD_TYPE}","Debug">>:__DEBUG>
    $<$<BOOL:${INTERNAL_DEBUG}>:__INTERNALDEBUG> # Internal debug activated.
    )
target_include_directories(CacheChangeTests PRIVATE
    ${PROJECT_SOURCE_DIR}/include ${PROJECT_BINARY_DIR}/include)
target_link_libraries(CacheChangeTests GTest::gtest)
add_gtest(CacheChangeTests SOURCES ${CACHECHANGETESTS_SOURCE})

add_executable(GuidUtilsTests ${GUID_UTILS_TESTS_SOURCE})
target_compile_definitions(GuidUtilsTests PRIVATE FASTRTPS_NO_LIB
    BOOST_ASIO_STANDALONE
    ASIO_STANDALONE
    $<$<AND:$<NOT:$<BOOL:${WIN32}>>,$<STREQUAL:"${CMAKE_BUILD_TYPE}","Debug">>:__DEBUG>
    $<$<BOOL:${INTERNAL_DEBUG}>:__INTERNALDEBUG> # Internal debug activated.
    )
target_include_directories(GuidUtilsTests PRIVATE
    ${PROJECT_SOURCE_DIR}/src/cpp
    ${PROJECT_SOURCE_DIR}/include ${PROJECT_BINARY_DIR}/include
    ${Asio_INCLUDE_DIR}
    $<$<BOOL:${ANDROID}>:${ANDROID_IFADDRS_INCLUDE_DIR}>)
target_link_libraries(GuidUtilsTests GTest::gtest)
if(MSVC OR MSVC_IDE)
    target_link_libraries(GuidUtilsTests ws2_32)
endif()
add_gtest(GuidUtilsTests SOURCES ${GUID_UTILS_TESTS_SOURCE})

add_executable(SequenceNumberTests ${SEQUENCENUMBERTESTS_SOURCE})
target_compile_definitions(SequenceNumberTests PRIVATE FASTRTPS_NO_LIB
    $<$<AND:$<NOT:$<BOOL:${WIN32}>>,$<STREQUAL:"${CMAKE_BUILD_TYPE}","Debug">>:__DEBUG>
    $<$<BOOL:${INTERNAL_DEBUG}>:__INTERNALDEBUG> # Internal debug activated.
    )
target_include_directories(SequenceNumberTests PRIVATE
    ${PROJECT_SOURCE_DIR}/include ${PROJECT_BINARY_DIR}/include)
target_link_libraries(SequenceNumberTests GTest::gtest)
add_gtest(SequenceNumberTests SOURCES ${SEQUENCENUMBERTESTS_SOURCE})

add_executable(PortParametersTests ${PORTPARAMETERSTESTS_SOURCE})
target_compile_definitions(PortParametersTests PRIVATE FASTRTPS_NO_LIB
    $<$<AND:$<NOT:$<BOOL:${WIN32}>>,$<STREQUAL:"${CMAKE_BUILD_TYPE}","Debug">>:__DEBUG>
    $<$<BOOL:${INTERNAL_DEBUG}>:__INTERNALDEBUG> # Internal debug activated.
    )
target_include_directories(PortParametersTests PRIVATE
    ${PROJECT_SOURCE_DIR}/include ${PROJECT_BINARY_DIR}/include
    ${PROJECT_SOURCE_DIR}/src/cpp)
target_link_libraries(PortParametersTests GTest::gtest)
add_gtest(PortParametersTests SOURCES ${PORTPARAMETERSTESTS_SOURCE} LABELS "NoMemoryCheck")

###################
# GuifPrefix test #
###################

set(GUIDPREFIXTESTS_SOURCE GuidPrefixTests.cpp)

add_executable(GuidPrefixTests ${GUIDPREFIXTESTS_SOURCE})
target_compile_definitions(GuidPrefixTests PRIVATE FASTRTPS_NO_LIB
    $<$<AND:$<NOT:$<BOOL:${WIN32}>>,$<STREQUAL:"${CMAKE_BUILD_TYPE}","Debug">>:__DEBUG>
    $<$<BOOL:${INTERNAL_DEBUG}>:__INTERNALDEBUG> # Internal debug activated.
    )
target_include_directories(GuidPrefixTests PRIVATE
    ${PROJECT_SOURCE_DIR}/include ${PROJECT_BINARY_DIR}/include
    ${PROJECT_SOURCE_DIR}/src/cpp)
target_link_libraries(GuidPrefixTests GTest::gtest)
add_gtest(GuidPrefixTests SOURCES ${GUIDPREFIXTESTS_SOURCE} LABELS "NoMemoryCheck")

#################
# EntityId test #
#################

set(ENTITYIDTESTS_SOURCE EntityIdTests.cpp)

add_executable(EntityIdTests ${ENTITYIDTESTS_SOURCE})
target_compile_definitions(EntityIdTests PRIVATE FASTRTPS_NO_LIB
    $<$<AND:$<NOT:$<BOOL:${WIN32}>>,$<STREQUAL:"${CMAKE_BUILD_TYPE}","Debug">>:__DEBUG>
    $<$<BOOL:${INTERNAL_DEBUG}>:__INTERNALDEBUG> # Internal debug activated.
    )
target_include_directories(EntityIdTests PRIVATE
    ${PROJECT_SOURCE_DIR}/include ${PROJECT_BINARY_DIR}/include
    ${PROJECT_SOURCE_DIR}/src/cpp)
target_link_libraries(EntityIdTests GTest::gtest)
add_gtest(EntityIdTests SOURCES ${ENTITYIDTESTS_SOURCE} LABELS "NoMemoryCheck")

#############
# Guid test #
#############

set(GUIDTESTS_SOURCE GuidTests.cpp)

add_executable(GuidTests ${GUIDTESTS_SOURCE})
target_compile_definitions(GuidTests PRIVATE FASTRTPS_NO_LIB
    $<$<AND:$<NOT:$<BOOL:${WIN32}>>,$<STREQUAL:"${CMAKE_BUILD_TYPE}","Debug">>:__DEBUG>
    $<$<BOOL:${INTERNAL_DEBUG}>:__INTERNALDEBUG> # Internal debug activated.
    )
target_include_directories(GuidTests PRIVATE
    ${PROJECT_SOURCE_DIR}/include ${PROJECT_BINARY_DIR}/include
    ${PROJECT_SOURCE_DIR}/src/cpp)
target_link_libraries(GuidTests GTest::gtest)
add_gtest(GuidTests SOURCES ${GUIDTESTS_SOURCE} LABELS "NoMemoryCheck")

if(ANDROID)
    set_property(TARGET CacheChangeTests PROPERTY CROSSCOMPILING_EMULATOR "adb;shell;cd;${CMAKE_CURRENT_BINARY_DIR};&&")
    set_property(TARGET EntityIdTests PROPERTY CROSSCOMPILING_EMULATOR "adb;shell;cd;${CMAKE_CURRENT_BINARY_DIR};&&")
    set_property(TARGET GuidTests PROPERTY CROSSCOMPILING_EMULATOR "adb;shell;cd;${CMAKE_CURRENT_BINARY_DIR};&&")
    set_property(TARGET GuidPrefixTests PROPERTY CROSSCOMPILING_EMULATOR "adb;shell;cd;${CMAKE_CURRENT_BINARY_DIR};&&")
    set_property(TARGET GuidUtilsTests PROPERTY CROSSCOMPILING_EMULATOR "adb;shell;cd;${CMAKE_CURRENT_BINARY_DIR};&&")
    set_property(TARGET PortParametersTests PROPERTY CROSSCOMPILING_EMULATOR "adb;shell;cd;${CMAKE_CURRENT_BINARY_DIR};&&")
    set_property(TARGET SequenceNumberTests PROPERTY CROSSCOMPILING_EMULATOR "adb;shell;cd;${CMAKE_CURRENT_BINARY_DIR};&&")
endif()
