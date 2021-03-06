#list( APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/libraries/fc/GitVersionGen" )
#include( GetGitRevisionDescription )
#get_git_head_revision(GIT_REFSPEC XMAX_GIT_REVISION_SHA)
#get_git_unix_timestamp(XMAX_GIT_REVISION_UNIX_TIMESTAMP)
#git_describe(XMAX_GIT_REVISION_DESCRIPTION --tags)
if(NOT XMAX_GIT_REVISION_DESCRIPTION)
    set(XMAX_GIT_REVISION_DESCRIPTION "unknown")
endif(NOT XMAX_GIT_REVISION_DESCRIPTION)

file(GLOB HEADERS "include/*.hpp")

set(sources
        key_conversion.cpp
        string_escape.cpp
        tempdir.cpp
        words.cpp
   ${HEADERS})

#configure_file("${CMAKE_CURRENT_SOURCE_DIR}/git_revision.cpp.in" "${CMAKE_CURRENT_BINARY_DIR}/git_revision.cpp" @ONLY)
#list(APPEND sources "${CMAKE_CURRENT_BINARY_DIR}/git_revision.cpp")

add_library( xmaxutilities
             ${sources}
             ${HEADERS} )
target_link_libraries( xmaxutilities fc WAST WASM )
target_include_directories( xmaxutilities
                            PUBLIC "include")
if (USE_PCH)
  set_target_properties(xmaxutilities PROPERTIES COTIRE_ADD_UNITY_BUILD FALSE)
  cotire(xmaxutilities)
endif(USE_PCH)

install( TARGETS
  xmaxutilities

   RUNTIME DESTINATION bin
   LIBRARY DESTINATION lib
   ARCHIVE DESTINATION lib
)
install( FILES ${HEADERS} DESTINATION "include" )
set_target_properties( xmaxutilities PROPERTIES PUBLIC_HEADER "${HEADERS}" FOLDER "1.Foundation")