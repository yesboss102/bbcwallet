cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB bbcwallet)

include_directories(${CMAKE_BINARY_DIR}
    src
    bbcwallet/external
    bbcwallet/include
    bbcwallet/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "1")

configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  ~/bbcwallet/external/miniupnpc/connecthostport.c
  ~/bbcwallet/external/miniupnpc/igd_desc_parse.c
  ~/bbcwallet/external/miniupnpc/minisoap.c
  ~/bbcwallet/external/miniupnpc/miniupnpc.c
  ~/bbcwallet/external/miniupnpc/miniwget.c
  ~/bbcwallet/external/miniupnpc/minixml.c
  ~/bbcwallet/external/miniupnpc/portlistingparse.c
  ~/bbcwallet/external/miniupnpc/receivedata.c
  ~/bbcwallet/external/miniupnpc/upnpcommands.c
  ~/bbcwallet/external/miniupnpc/upnpreplyparse.c
  ~/bbcwallet/src/Common/Base58.cpp
  ~/bbcwallet/src/Common/CommandLine.cpp
  ~/bbcwallet/src/Common/Util.cpp
  ~/bbcwallet/src/Common/StringTools.cpp
  ~/bbcwallet/src/Common/JsonValue.cpp
  ~/bbcwallet/src/Common/ConsoleTools.cpp
  ~/bbcwallet/src/Common/MemoryInputStream.cpp
  ~/bbcwallet/src/Common/PathTools.cpp
  ~/bbcwallet/src/Common/StdInputStream.cpp
  ~/bbcwallet/src/Common/StdOutputStream.cpp
  ~/bbcwallet/src/Common/StreamTools.cpp
  ~/bbcwallet/src/Common/StringOutputStream.cpp
  ~/bbcwallet/src/Common/StringView.cpp
  ~/bbcwallet/src/Common/VectorOutputStream.cpp
  ~/bbcwallet/src/crypto/blake256.c
  ~/bbcwallet/src/crypto/chacha8.c
  ~/bbcwallet/src/crypto/crypto-ops-data.c
  ~/bbcwallet/src/crypto/crypto-ops.c
  ~/bbcwallet/src/crypto/crypto.cpp
  ~/bbcwallet/src/crypto/groestl.c
  ~/bbcwallet/src/crypto/hash-extra-blake.c
  ~/bbcwallet/src/crypto/hash-extra-groestl.c
  ~/bbcwallet/src/crypto/hash-extra-jh.c
  ~/bbcwallet/src/crypto/hash-extra-skein.c
  ~/bbcwallet/src/crypto/hash.c
  ~/bbcwallet/src/crypto/jh.c
  ~/bbcwallet/src/crypto/keccak.c
  ~/bbcwallet/src/crypto/oaes_lib.c
  ~/bbcwallet/src/crypto/random.c
  ~/bbcwallet/src/crypto/skein.c
  ~/bbcwallet/src/crypto/slow-hash.c
  ~/bbcwallet/src/crypto/slow-hash.cpp
  ~/bbcwallet/src/crypto/tree-hash.c
  ~/bbcwallet/src/CryptoNoteCore/BlockchainIndices.cpp
  ~/bbcwallet/src/CryptoNoteCore/BlockchainMessages.cpp
  ~/bbcwallet/src/CryptoNoteCore/BlockIndex.cpp
  ~/bbcwallet/src/CryptoNoteCore/CoreConfig.cpp
  ~/bbcwallet/src/CryptoNoteCore/CryptoNoteBasic.cpp
  ~/bbcwallet/src/CryptoNoteCore/CryptoNoteTools.cpp
  ~/bbcwallet/src/CryptoNoteCore/Currency.cpp
  ~/bbcwallet/src/CryptoNoteCore/MinerConfig.cpp
  ~/bbcwallet/src/CryptoNoteCore/Transaction.cpp
  ~/bbcwallet/src/CryptoNoteCore/Account.cpp
  ~/bbcwallet/src/CryptoNoteCore/Blockchain.cpp
  ~/bbcwallet/src/CryptoNoteCore/Checkpoints.cpp
  ~/bbcwallet/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  ~/bbcwallet/src/CryptoNoteCore/Core.cpp
  ~/bbcwallet/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  ~/bbcwallet/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  ~/bbcwallet/src/CryptoNoteCore/Difficulty.cpp
  ~/bbcwallet/src/CryptoNoteCore/IBlock.cpp
  ~/bbcwallet/src/CryptoNoteCore/Miner.cpp
  ~/bbcwallet/src/CryptoNoteCore/TransactionExtra.cpp
  ~/bbcwallet/src/CryptoNoteCore/TransactionPool.cpp
  ~/bbcwallet/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  ~/bbcwallet/src/CryptoNoteCore/TransactionUtils.cpp
  ~/bbcwallet/src/InProcessNode/InProcessNode.cpp
  ~/bbcwallet/src/InProcessNode/InProcessNodeErrors.cpp
  ~/bbcwallet/src/NodeRpcProxy/NodeErrors.cpp
  ~/bbcwallet/src/NodeRpcProxy/NodeRpcProxy.cpp
  ~/bbcwallet/src/P2p/NetNodeConfig.cpp
  ~/bbcwallet/src/Serialization/BinaryInputStreamSerializer.cpp
  ~/bbcwallet/src/Serialization/BinaryOutputStreamSerializer.cpp
  ~/bbcwallet/src/Serialization/JsonInputValueSerializer.cpp
  ~/bbcwallet/src/Serialization/JsonOutputStreamSerializer.cpp
  ~/bbcwallet/src/Serialization/KVBinaryInputStreamSerializer.cpp
  ~/bbcwallet/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  ~/bbcwallet/src/Serialization/SerializationOverloads.cpp
  ~/bbcwallet/src/Transfers/BlockchainSynchronizer.cpp
  ~/bbcwallet/src/Transfers/SynchronizationState.cpp
  ~/bbcwallet/src/Transfers/TransfersConsumer.cpp
  ~/bbcwallet/src/Transfers/TransfersContainer.cpp
  ~/bbcwallet/src/Transfers/TransfersSubscription.cpp
  ~/bbcwallet/src/Transfers/TransfersSynchronizer.cpp
  ~/bbcwallet/src/Wallet/LegacyKeysImporter.cpp
  ~/bbcwallet/src/Wallet/WalletAsyncContextCounter.cpp
  ~/bbcwallet/src/Wallet/WalletErrors.cpp
  ~/bbcwallet/src/WalletLegacy/KeysStorage.cpp
  ~/bbcwallet/src/WalletLegacy/WalletLegacy.cpp
  ~/bbcwallet/src/WalletLegacy/WalletHelper.cpp
  ~/bbcwallet/src/WalletLegacy/WalletLegacySerializer.cpp
  ~/bbcwallet/src/WalletLegacy/WalletLegacySerialization.cpp
  ~/bbcwallet/src/WalletLegacy/WalletTransactionSender.cpp
  ~/bbcwallet/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  ~/bbcwallet/src/WalletLegacy/WalletUserTransactionsCache.cpp
  ~/bbcwallet/src/System/ContextGroup.cpp
  ~/bbcwallet/src/System/Event.cpp
  ~/bbcwallet/src/System/EventLock.cpp
  ~/bbcwallet/src/System/InterruptedException.cpp
  ~/bbcwallet/src/System/Ipv4Address.cpp
  ~/bbcwallet/src/System/TcpStream.cpp
  ~/bbcwallet/src/HTTP/HttpRequest.cpp
  ~/bbcwallet/src/HTTP/HttpParser.cpp
  ~/bbcwallet/src/HTTP/HttpParserErrorCodes.cpp
  ~/bbcwallet/src/HTTP/HttpResponse.cpp
  ~/bbcwallet/src/Rpc/HttpClient.cpp
  ~/bbcwallet/src/Rpc/JsonRpc.cpp
  ~/bbcwallet/src/P2p/NetNode.cpp
  ~/bbcwallet/src/P2p/LevinProtocol.cpp
  ~/bbcwallet/src/P2p/NetNodeConfig.cpp
  ~/bbcwallet/src/P2p/PeerListManager.cpp
  ~/bbcwallet/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  ~/bbcwallet/src/Logging/ILogger.cpp
  ~/bbcwallet/src/Logging/LoggerGroup.cpp
  ~/bbcwallet/src/Logging/CommonLogger.cpp
  ~/bbcwallet/src/Logging/LoggerManager.cpp
  ~/bbcwallet/src/Logging/FileLogger.cpp
  ~/bbcwallet/src/Logging/StreamLogger.cpp
  ~/bbcwallet/src/Logging/ConsoleLogger.cpp
  ~/bbcwallet/src/Logging/LoggerMessage.cpp
  ~/bbcwallet/src/Logging/LoggerRef.cpp
  ~/bbcwallet/src/BlockchainExplorer/BlockchainExplorer.cpp
  ~/bbcwallet/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  ~/bbcwallet/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(bbcwallet/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/bbcwalletwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} bbcwallet/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(./src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES ./src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
