get_filename_component(DIRNAME "${CMAKE_CURRENT_SOURCE_DIR}" NAME)
set(EXEC_NAME "${PROJECT_NAME}_${DIRNAME}")

###########
# Library #
###########

include(CheckIncludeFile)

find_package(BISON)
find_package(FLEX)

BISON_TARGET(BcParser bc.y "${CMAKE_CURRENT_BINARY_DIR}/bc_parser.c")
FLEX_TARGET(BcScanner bc.1  "${CMAKE_CURRENT_BINARY_DIR}/bc_lexer.c")
ADD_FLEX_BISON_DEPENDENCY(BcScanner BcParser)
include_directories(${CMAKE_CURRENT_BINARY_DIR})
add_executable("${EXEC_NAME}"
        "${CMAKE_CURRENT_BINARY_DIR}/bc_parser.c"
        ${BISON_MyParser_OUTPUTS}
        ${FLEX_MyScanner_OUTPUTS}
)
target_link_libraries("${EXEC_NAME}" ${FLEX_LIBRARIES})
