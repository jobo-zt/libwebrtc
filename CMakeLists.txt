cmake_minimum_required(VERSION 3.0)

project(libwebrtc)

set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/webrtc)

option(BUILD_TEST "Build Test" on)

if(NOT DEFINED CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE DEBUG)
endif()
if(DEFINED WIN32)
add_definitions(-DWEBRTC_WIN)
endif()

if(NOT DEFINED WIN32)
add_compile_options(-fvisibility=hidden)
add_definitions(-DWEBRTC_POSIX)
endif()

set(SRCS
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/modules/audio_processing/aecm/aecm_core.cc   
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/modules/audio_processing/aecm/aecm_core_c.cc	
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/modules/audio_processing/aecm/echo_control_mobile.cc        
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/modules/audio_processing/utility/delay_estimator_wrapper.cc 
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/modules/audio_processing/utility/delay_estimator.cc   
	# _M_ARM 
	#${CMAKE_CURRENT_SOURCE_DIR}/webrtc/modules/audio_processing/aecm/aecm_core_neon.cc            
	#${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/cross_correlation_neon.c    
	#${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/downsample_fast_neon.c      
	#${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/min_max_operations_neon.c
	#
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/third_party/ooura/fft_size_256/fft4g.cc       
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/third_party/spl_sqrt_floor/spl_sqrt_floor.c   
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/ring_buffer.c                                 
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/complex_bit_reverse.c       
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/complex_fft.c               
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/copy_set_operations.c       
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/cross_correlation.c         
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/division_operations.c       
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/downsample_fast.c           
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/energy.c                    
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/get_scaling_square.c        
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/min_max_operations.c        
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/randomization_functions.c   
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/real_fft.c                  
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/spl_inl.c   
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/spl_init.c 	
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/spl_sqrt.c                  
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/common_audio/signal_processing/vector_scaling_operations.c
	
	${CMAKE_CURRENT_SOURCE_DIR}/webrtc/rtc_base/checks.cc
)

#add_library(libwebrtc SHARED ${SRCS})
#target_link_libraries(libwebrtc)
add_library(libwebrtc STATIC ${SRCS})


if(BUILD_TEST)
    #add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/Test)
endif()