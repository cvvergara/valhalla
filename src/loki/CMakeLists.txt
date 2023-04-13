file(GLOB headers ${VALHALLA_SOURCE_DIR}/valhalla/loki/*.h)

set(sources
  height_action.cc
  isochrone_action.cc
  locate_action.cc
  matrix_action.cc
  node_search.cc
  polygon_search.cc
  reach.cc
  route_action.cc
  search.cc
  status_action.cc
  trace_route_action.cc
  transit_available_action.cc
  worker.cc
  )

# treat date library as system
set(system_includes ${VALHALLA_SOURCE_DIR}/third_party/date/include)
if(APPLE)
  list(APPEND system_includes ${VALHALLA_SOURCE_DIR}/third_party/date/include/date)
endif()

valhalla_module(NAME loki
  SOURCES ${sources}
  HEADERS ${headers}
  INCLUDE_DIRECTORIES
    PUBLIC
      ${VALHALLA_SOURCE_DIR}
      ${VALHALLA_SOURCE_DIR}/valhalla
      $<$<BOOL:${WIN32}>:${VALHALLA_SOURCE_DIR}/third_party/dirent/include>
    PRIVATE
      ${VALHALLA_SOURCE_DIR}/third_party/rapidjson/include
      ${CMAKE_BINARY_DIR}
  SYSTEM_INCLUDE_DIRECTORIES
    PUBLIC
      ${system_includes}
  DEPENDS
    valhalla::skadi
    valhalla::sif
    valhalla::proto
    ${valhalla_protobuf_targets}
    Boost::boost
    libprime_server)
