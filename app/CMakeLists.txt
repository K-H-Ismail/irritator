add_executable(app src/main.cpp)

target_link_libraries(app libirritator threads)

set_target_properties(app PROPERTIES
  OUTPUT_NAME "irritator-${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}"
  CXX_STANDARD 20)

target_compile_options(app
  PRIVATE
  $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:GNU>>:
      -pipe -march=native
      $<$<CONFIG:Debug>:-Og -Wall -Wextra>>
  $<$<CXX_COMPILER_ID:MSVC>:
      /EHsc /bigobj /Zc:__cplusplus /std:c++latest
      $<$<CONFIG:Debug>:/Od /W3 /Zi>>)

target_compile_definitions(app
  PRIVATE
  $<$<BOOL:${WITH_DEBUG}>:IRRITATOR_ENABLE_DEBUG>
  $<$<CXX_COMPILER_ID:MSVC>:_CRT_SECURE_NO_WARNINGS>
  $<$<CXX_COMPILER_ID:MSVC>:_SCL_SECURE_NO_WARNINGS>
  VERSION_MAJOR=${PROJECT_VERSION_MAJOR}
  VERSION_MINOR=${PROJECT_VERSION_MINOR}
  VERSION_PATCH=${PROJECT_VERSION_PATCH}
  VERSION_TWEAK=${PROJECT_VERSION_TWEAK})

install(TARGETS app
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

if (WITH_GUI)
    add_subdirectory(gui)
endif ()
