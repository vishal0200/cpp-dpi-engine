cmake_minimum_required(VERSION 3.16)
project(PacketAnalyzer VERSION 1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Hardware Optimization Flags (Full Throttle)
if(NOT MSVC)
    add_compile_options(-O3 -march=native -flto)
    add_link_options(-flto)
endif()

include_directories(${CMAKE_SOURCE_DIR}/include)

# 1. SIMPLE ANALYZER (Basic Tool)
add_executable(packet_analyzer
    src/main_simple.cpp
    src/pcap_reader.cpp
    src/packet_parser.cpp
    src/sni_extractor.cpp
)

# 2. MODULAR DPI ENGINE (Production Style)
# Removed src/dpi_mt.cpp from here to fix the "multiple definition of main" error
add_executable(dpi_engine
    src/main_dpi.cpp
    src/dpi_engine.cpp
    src/load_balancer.cpp
    src/fast_path.cpp
    src/connection_tracker.cpp
    src/rule_manager.cpp
    src/sni_extractor.cpp
    src/types.cpp
    src/pcap_reader.cpp
    src/packet_parser.cpp
)

# 3. STANDALONE MT ENGINE (Performance/Concept Demo)
add_executable(dpi_mt_standalone
    src/dpi_mt.cpp
    src/sni_extractor.cpp
    src/pcap_reader.cpp
    src/packet_parser.cpp
    src/types.cpp
)