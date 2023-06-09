cmake_minimum_required(VERSION 3.19)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
find_package(ESMF 8.4.0 MODULE REQUIRED)

if(NOT DEFINED CMAKE_Fortran_COMPILER)
  set(CMAKE_Fortran_COMPILER "${ESMF_F90COMPILER}")
endif()
if(NOT DEFINED CMAKE_Fortran_FLAGS)
  set(CMAKE_Fortran_FLAGS "${ESMF_F90COMPILEOPTS}")
endif()

project(Lumo
        VERSION 1.0
        LANGUAGES Fortran)

add_library(Lumo lumo.F90)
target_link_libraries(Lumo ESMF)

install(TARGETS Lumo
        EXPORT Lumo
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        INCLUDES DESTINATION mod
)
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/
        DESTINATION mod
        FILES_MATCHING
        PATTERN "*.mod"
        PATTERN "CMakeFiles" EXCLUDE
)
install(EXPORT Lumo DESTINATION cmake)
