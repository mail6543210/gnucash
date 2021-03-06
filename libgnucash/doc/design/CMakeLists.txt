
SET(gnucash_design_TEXINFOS
  component-manager.texi
  concept-index.texi
  engine.texi
  fdl.texi
  function-index.texi
  intro.texi
  register.texi
  reports.texi
  top-level.texi
  type-index.texi
  user-preferences.texi
)

  EXECUTE_PROCESS(COMMAND date +%e OUTPUT_VARIABLE CURRENT_DATE OUTPUT_STRIP_TRAILING_WHITESPACE)
  EXECUTE_PROCESS(COMMAND date +%B OUTPUT_VARIABLE CURRENT_MONTH OUTPUT_STRIP_TRAILING_WHITESPACE)
  EXECUTE_PROCESS(COMMAND date +%Y OUTPUT_VARIABLE CURRENT_YEAR OUTPUT_STRIP_TRAILING_WHITESPACE)
SET(UPDATED "${CURRENT_DATE} ${CURRENT_MONTH} ${CURRENT_YEAR}")
SET(UPDATED_MONTH "${CURRENT_MONTH} ${CURRENT_YEAR}")

SET(VERSION_TEXI_IN
  "@set UPDATED ${UPDATED}
@set UPDATED-MONTH ${UPDATED_MONTH}
@set EDITION ${VERSION}
@set VERSION ${VERSION}
")

FILE(WRITE ${CMAKE_CURRENT_BINARY_DIR}/version.texi ${VERSION_TEXI_IN})
FILE(WRITE ${CMAKE_CURRENT_BINARY_DIR}/stamp-vti ${VERSION_TEXI_IN})

FILE(TO_NATIVE_PATH "${CMAKE_CURRENT_BINARY_DIR}" TEXI_BINARY_DIR)
FILE(TO_NATIVE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/gnucash-design.texi" TEXI_SOURCE_FILE)
find_program(MAKEINFO makeinfo)
if (NOT ${MAKEINFO} STREQUAL "MAKEINFO-NOTFOUND")

  IF(${CMAKE_VERSION} VERSION_GREATER 3.1)
    add_custom_command(
      OUTPUT gnucash-design.info
      COMMAND ${MAKEINFO} -I ${TEXI_BINARY_DIR} ${TEXI_SOURCE_FILE}
      DEPENDS ${gnucash_design_TEXINFOS}
      )
  ELSE()
    add_custom_command(
      OUTPUT gnucash-design.info
      COMMAND ${MAKEINFO} -I ${TEXI_BINARY_DIR} ${TEXI_SOURCE_FILE}
      DEPENDS ${gnucash_design_TEXINFOS}
      )
  ENDIF()
endif (NOT ${MAKEINFO} STREQUAL "MAKEINFO-NOTFOUND")
dist_add_generated (${BUILDING_FROM_VCS} gnucash-design.info)

SET_DIST_LIST(doc_design_DIST CMakeLists.txt gnucash-design.texi ${gnucash_design_TEXINFOS})
