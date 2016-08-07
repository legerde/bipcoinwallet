cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    ../bipcoin/external
    ../bipcoin/include
    ../bipcoin/src)

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

set(VERSION "")
configure_file("../bipcoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  ../bipcoin/external/miniupnpc/connecthostport.c
  ../bipcoin/external/miniupnpc/igd_desc_parse.c
  ../bipcoin/external/miniupnpc/minisoap.c
  ../bipcoin/external/miniupnpc/miniupnpc.c
  ../bipcoin/external/miniupnpc/miniwget.c
  ../bipcoin/external/miniupnpc/minixml.c
  ../bipcoin/external/miniupnpc/portlistingparse.c
  ../bipcoin/external/miniupnpc/receivedata.c
  ../bipcoin/external/miniupnpc/upnpcommands.c
  ../bipcoin/external/miniupnpc/upnpreplyparse.c
  ../bipcoin/src/Common/Base58.cpp
  ../bipcoin/src/Common/CommandLine.cpp
  ../bipcoin/src/Common/Util.cpp
  ../bipcoin/src/Common/StringTools.cpp
  ../bipcoin/src/Common/JsonValue.cpp
  ../bipcoin/src/Common/ConsoleTools.cpp
  ../bipcoin/src/Common/MemoryInputStream.cpp
  ../bipcoin/src/Common/PathTools.cpp
  ../bipcoin/src/Common/StdInputStream.cpp
  ../bipcoin/src/Common/StdOutputStream.cpp
  ../bipcoin/src/Common/StreamTools.cpp
  ../bipcoin/src/Common/StringOutputStream.cpp
  ../bipcoin/src/Common/StringView.cpp
  ../bipcoin/src/Common/VectorOutputStream.cpp
  ../bipcoin/src/crypto/blake256.c
  ../bipcoin/src/crypto/chacha8.c
  ../bipcoin/src/crypto/crypto-ops-data.c
  ../bipcoin/src/crypto/crypto-ops.c
  ../bipcoin/src/crypto/crypto.cpp
  ../bipcoin/src/crypto/groestl.c
  ../bipcoin/src/crypto/hash-extra-blake.c
  ../bipcoin/src/crypto/hash-extra-groestl.c
  ../bipcoin/src/crypto/hash-extra-jh.c
  ../bipcoin/src/crypto/hash-extra-skein.c
  ../bipcoin/src/crypto/hash.c
  ../bipcoin/src/crypto/jh.c
  ../bipcoin/src/crypto/keccak.c
  ../bipcoin/src/crypto/oaes_lib.c
  ../bipcoin/src/crypto/random.c
  ../bipcoin/src/crypto/skein.c
  ../bipcoin/src/crypto/slow-hash.c
  ../bipcoin/src/crypto/slow-hash.cpp
  ../bipcoin/src/crypto/tree-hash.c
  ../bipcoin/src/CryptoNoteCore/BlockchainIndices.cpp
  ../bipcoin/src/CryptoNoteCore/BlockchainMessages.cpp
  ../bipcoin/src/CryptoNoteCore/BlockIndex.cpp
  ../bipcoin/src/CryptoNoteCore/CoreConfig.cpp
  ../bipcoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  ../bipcoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  ../bipcoin/src/CryptoNoteCore/Currency.cpp
  ../bipcoin/src/CryptoNoteCore/MinerConfig.cpp
  ../bipcoin/src/CryptoNoteCore/Transaction.cpp
  ../bipcoin/src/CryptoNoteCore/Account.cpp
  ../bipcoin/src/CryptoNoteCore/Blockchain.cpp
  ../bipcoin/src/CryptoNoteCore/Checkpoints.cpp
  ../bipcoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  ../bipcoin/src/CryptoNoteCore/Core.cpp
  ../bipcoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  ../bipcoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  ../bipcoin/src/CryptoNoteCore/Difficulty.cpp
  ../bipcoin/src/CryptoNoteCore/IBlock.cpp
  ../bipcoin/src/CryptoNoteCore/Miner.cpp
  ../bipcoin/src/CryptoNoteCore/TransactionExtra.cpp
  ../bipcoin/src/CryptoNoteCore/TransactionPool.cpp
  ../bipcoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  ../bipcoin/src/CryptoNoteCore/TransactionUtils.cpp
  ../bipcoin/src/InProcessNode/InProcessNode.cpp
  ../bipcoin/src/InProcessNode/InProcessNodeErrors.cpp
  ../bipcoin/src/NodeRpcProxy/NodeErrors.cpp
  ../bipcoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  ../bipcoin/src/P2p/NetNodeConfig.cpp
  ../bipcoin/src/Serialization/BinaryInputStreamSerializer.cpp
  ../bipcoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  ../bipcoin/src/Serialization/JsonInputValueSerializer.cpp
  ../bipcoin/src/Serialization/JsonOutputStreamSerializer.cpp
  ../bipcoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  ../bipcoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  ../bipcoin/src/Serialization/SerializationOverloads.cpp
  ../bipcoin/src/Transfers/BlockchainSynchronizer.cpp
  ../bipcoin/src/Transfers/SynchronizationState.cpp
  ../bipcoin/src/Transfers/TransfersConsumer.cpp
  ../bipcoin/src/Transfers/TransfersContainer.cpp
  ../bipcoin/src/Transfers/TransfersSubscription.cpp
  ../bipcoin/src/Transfers/TransfersSynchronizer.cpp
  ../bipcoin/src/Wallet/LegacyKeysImporter.cpp
  ../bipcoin/src/Wallet/WalletAsyncContextCounter.cpp
  ../bipcoin/src/Wallet/WalletErrors.cpp
  ../bipcoin/src/WalletLegacy/KeysStorage.cpp
  ../bipcoin/src/WalletLegacy/WalletLegacy.cpp
  ../bipcoin/src/WalletLegacy/WalletHelper.cpp
  ../bipcoin/src/WalletLegacy/WalletLegacySerializer.cpp
  ../bipcoin/src/WalletLegacy/WalletLegacySerialization.cpp
  ../bipcoin/src/WalletLegacy/WalletTransactionSender.cpp
  ../bipcoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  ../bipcoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  ../bipcoin/src/System/ContextGroup.cpp
  ../bipcoin/src/System/Event.cpp
  ../bipcoin/src/System/EventLock.cpp
  ../bipcoin/src/System/InterruptedException.cpp
  ../bipcoin/src/System/Ipv4Address.cpp
  ../bipcoin/src/System/TcpStream.cpp
  ../bipcoin/src/HTTP/HttpRequest.cpp
  ../bipcoin/src/HTTP/HttpParser.cpp
  ../bipcoin/src/HTTP/HttpParserErrorCodes.cpp
  ../bipcoin/src/HTTP/HttpResponse.cpp
  ../bipcoin/src/Rpc/HttpClient.cpp
  ../bipcoin/src/Rpc/JsonRpc.cpp
  ../bipcoin/src/P2p/NetNode.cpp
  ../bipcoin/src/P2p/LevinProtocol.cpp
  ../bipcoin/src/P2p/NetNodeConfig.cpp
  ../bipcoin/src/P2p/PeerListManager.cpp
  ../bipcoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  ../bipcoin/src/Logging/ILogger.cpp
  ../bipcoin/src/Logging/LoggerGroup.cpp
  ../bipcoin/src/Logging/CommonLogger.cpp
  ../bipcoin/src/Logging/LoggerManager.cpp
  ../bipcoin/src/Logging/FileLogger.cpp
  ../bipcoin/src/Logging/StreamLogger.cpp
  ../bipcoin/src/Logging/ConsoleLogger.cpp
  ../bipcoin/src/Logging/LoggerMessage.cpp
  ../bipcoin/src/Logging/LoggerRef.cpp
  ../bipcoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  ../bipcoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  ../bipcoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(../bipcoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ../bipcoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "BipCoin GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME BipCoinWallet)
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

include_directories(../bipcoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES ../bipcoin/src/Platform/${PLATFORM_DIR}/System/*)
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
