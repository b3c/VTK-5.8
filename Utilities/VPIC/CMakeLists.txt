 
project(VPIC)

SET(VPIC_BUILD_SHARED_LIBS ${BUILD_SHARED_LIBS})

CONFIGURE_FILE (${VPIC_SOURCE_DIR}/VPICDefinition.h.in
                ${VPIC_BINARY_DIR}/VPICDefinition.h)
 
SET(VPIC_SOURCES
  VPICDataSet.cxx
  VPICDefinition.cxx
  VPICGlobal.cxx
  VPICHeader.cxx
  VPICPart.cxx
  VPICView.cxx
  GridExchange.cxx
)

IF(VTK_USE_MPI)
  INCLUDE_DIRECTORIES("${MPI_INCLUDE_PATH}")
  ADD_DEFINITIONS("-DUSE_PARALLEL_VPIC -DMPICH_IGNORE_CXX_SEEK")
ENDIF(VTK_USE_MPI)

INCLUDE_DIRECTORIES (${VPIC_SOURCE_DIR} ${VPIC_BINARY_DIR})

VTK_ADD_LIBRARY(VPIC ${VPIC_SOURCES})
TARGET_LINK_LIBRARIES(VPIC vtksys )
IF(VTK_USE_MPI)
TARGET_LINK_LIBRARIES(VPIC ${MPI_LIBRARY})
  IF(MPI_EXTRA_LIBRARY)
    TARGET_LINK_LIBRARIES(VPIC ${MPI_EXTRA_LIBRARY})
  ENDIF(MPI_EXTRA_LIBRARY)
ENDIF(VTK_USE_MPI)

# Apply user-defined properties to the library target. Such as SO Version
IF(VTK_LIBRARY_PROPERTIES)
  SET_TARGET_PROPERTIES(VPIC PROPERTIES ${VTK_LIBRARY_PROPERTIES})
ENDIF(VTK_LIBRARY_PROPERTIES)

IF(NOT VTK_INSTALL_NO_LIBRARIES)
  INSTALL(TARGETS VPIC
    EXPORT ${VTK_INSTALL_EXPORT_NAME}
    RUNTIME DESTINATION ${VTK_INSTALL_BIN_DIR_CM24} COMPONENT RuntimeLibraries
    LIBRARY DESTINATION ${VTK_INSTALL_LIB_DIR_CM24} COMPONENT RuntimeLibraries
    ARCHIVE DESTINATION ${VTK_INSTALL_LIB_DIR_CM24} COMPONENT Development)
ENDIF(NOT VTK_INSTALL_NO_LIBRARIES)

IF(NOT VTK_INSTALL_NO_DEVELOPMENT)
  INSTALL(FILES
    ${VPIC_SOURCE_DIR}/VPICDataSet.h
    ${VPIC_SOURCE_DIR}/VPICGlobal.h
    ${VPIC_SOURCE_DIR}/VPICHeader.h
    ${VPIC_SOURCE_DIR}/VPICPart.h
    ${VPIC_SOURCE_DIR}/VPICView.h
    ${VPIC_SOURCE_DIR}/GridExchange.h
    ${VPIC_BINARY_DIR}/VPICDefinition.h
    DESTINATION ${VTK_INSTALL_INCLUDE_DIR_CM24}/VPIC
    COMPONENT Development)
ENDIF(NOT VTK_INSTALL_NO_DEVELOPMENT)
