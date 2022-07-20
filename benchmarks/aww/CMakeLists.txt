add_executable(aww)

target_include_directories(aww
  PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}
)

target_sources(aww
  PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/aww_data/aww_input_data.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/aww_data/aww_input_data.h
  ${CMAKE_CURRENT_SOURCE_DIR}/aww_data/aww_model_data.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/aww_data/aww_model_data.h
  ${CMAKE_CURRENT_SOURCE_DIR}/aww_data/aww_model_settings.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/aww_data/aww_model_settings.h
  ${CMAKE_CURRENT_SOURCE_DIR}/aww_data/aww_output_data_ref.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/aww_data/aww_output_data_ref.h
  ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
)

target_link_libraries(aww
  ${CMAKE_PROJECT_NAME}
)
