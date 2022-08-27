#
# Copyright (C) 2021-2022 Chair of Electronic Design Automation, TUM.
#
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the License); you may
# not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an AS IS BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

cmake_minimum_required(VERSION 3.10)

message("CMake Version ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}.${CMAKE_PATCH_VERSION}")

project(tflm LANGUAGES C CXX ASM DESCRIPTION "TFLM base project.")

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 11)

set(CMAKE_VERBOSE_MAKEFILE ON)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -O0 -g -ggdb")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0 -g -ggdb")
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -Ofast")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Ofast")

add_library(${CMAKE_PROJECT_NAME} STATIC)

target_include_directories(${CMAKE_PROJECT_NAME} PUBLIC
  ${CMAKE_SOURCE_DIR}
  ${CMAKE_SOURCE_DIR}/third_party/ruy
  ${CMAKE_SOURCE_DIR}/third_party/gemmlowp
  ${CMAKE_SOURCE_DIR}/third_party/kissfft
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include
)

set(TFLM_SRCS
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/c/common.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/c/c_api_types.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/c/common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/c/builtin_op_data.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/builtin_ops.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/schema/schema_generated.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/schema/schema_utils.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/schema/schema_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/context_util.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_resource_variable.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/fake_micro_context.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/test_helpers.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_planner/micro_memory_planner.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_planner/linear_memory_planner.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_planner/memory_plan_struct.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_planner/linear_memory_planner.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_planner/non_persistent_buffer_planner_shim.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_planner/greedy_memory_planner.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_planner/non_persistent_buffer_planner_shim.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_planner/greedy_memory_planner.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_helpers.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_allocator.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/system_setup.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/memory_helpers.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/mock_micro_graph.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/system_setup.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/flatbuffer_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/fake_micro_context.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/recording_micro_allocator.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_mutable_op_resolver.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/debug_log.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_profiler.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/test_helpers.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_resource_variable.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_context.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_profiler.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_utils.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_interpreter.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_allocation_info.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_string.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/mock_micro_graph.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/compatibility.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/flatbuffer_utils.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_error_reporter.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_time.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_allocation_info.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/single_arena_buffer_allocator.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/single_arena_buffer_allocator.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/persistent_arena_buffer_allocator.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/ibuffer_allocator.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/persistent_arena_buffer_allocator.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/recording_single_arena_buffer_allocator.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/recording_single_arena_buffer_allocator.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/non_persistent_arena_buffer_allocator.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/arena_allocator/non_persistent_arena_buffer_allocator.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_string.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/all_ops_resolver.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_graph.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/all_ops_resolver.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/recording_micro_interpreter.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_context.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_allocator.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_error_reporter.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_interpreter.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_time.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/test_helper_custom_ops.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_graph.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/recording_micro_allocator.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/hard_swish.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/add_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/detection_postprocess_flexbuffers_generated_data.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/fully_connected.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/lstm_eval.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/dequantize.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/kernel_util.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/concatenation.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/space_to_batch_nd.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/resize_nearest_neighbor.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/softmax.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/pack.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/broadcast_args.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/maximum_minimum.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/mul_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/quantize.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/hard_swish_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/softmax_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/cumsum.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/dequantize.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/prelu.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/pad.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/floor.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/mul.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/split_v.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/svdf_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/logistic.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/shape.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/micro_tensor_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/conv_test.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/batch_to_space_nd.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/circular_buffer_flexbuffers_generated_data.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/micro_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/kernel_runner.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/leaky_relu.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/sub.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/logistic.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/unidirectional_sequence_lstm.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/floor_div.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/kernel_runner.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/hard_swish.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/add.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/select.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/ceil.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/svdf.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/strided_slice.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/depthwise_conv_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/logical.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/fully_connected.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/conv.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/circular_buffer.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/pooling_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/micro_ops.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/kernel_util.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/assign_variable.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/sub_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/reduce.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/unpack.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/ethosu.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/activations_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/ethosu.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/conv.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/squared_difference.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/l2norm.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/depthwise_conv.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/l2_pool_2d.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/round.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/tanh.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/add_n.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/dequantize_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/broadcast_to.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/circular_buffer.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/space_to_depth.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/add.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/quantize_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/logical.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/split.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/if.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/expand_dims.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/logical_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/lstm_eval.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/sub.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/prelu_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/slice.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/gather.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/squeeze.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/floor_mod.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/depth_to_space.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/quantize.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/mul.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/fully_connected_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/gather_nd.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/reduce_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/conv_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/mirror_pad.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/activations.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/micro_tensor_utils.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/activations.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/fill.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/resize_bilinear.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/svdf.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/call_once.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/prelu.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/reshape.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/lstm_shared.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/zeros_like.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/neg.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/elementwise.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/read_variable.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/div.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/pooling.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/softmax.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/while.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/exp.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/reduce.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/detection_postprocess.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/depthwise_conv.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/circular_buffer_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/leaky_relu_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/activation_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/arg_min_max.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/comparisons.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/log_softmax.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/cast.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/leaky_relu.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/transpose_conv.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/pooling.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/elu.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/var_handle.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/unidirectional_sequence_lstm_test_config.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/transpose.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/kernels/logistic_common.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/debug_log.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_op_resolver.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/micro_arena_constants.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/micro/test_helper_custom_ops.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/core/api/error_reporter.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/core/api/error_reporter.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/core/api/op_resolver.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/core/api/flatbuffer_conversions.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/core/api/tensor_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/core/api/op_resolver.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/core/api/tensor_utils.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/core/api/flatbuffer_conversions.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/op_macros.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/kernel_util.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/kernel_util.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/tensor_ctypes.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/strided_slice_logic.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/common.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/types.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/min.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/optimized/neon_check.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/portable_tensor.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/quantization_util.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/portable_tensor_utils_impl.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/softmax.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/slice.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/log_softmax.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/process_broadcast_shapes.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/broadcast_to.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/transpose.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/depthwiseconv_float.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/dequantize.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/prelu.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/logistic.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/comparisons.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/depthwiseconv_uint8.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/space_to_depth.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/ceil.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/sub.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/transpose_conv.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/depth_to_space.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/hard_swish.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/strided_slice.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/fully_connected.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/floor_mod.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/batch_to_space_nd.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/binary_function.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/requantize.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/conv.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/broadcast_args.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/concatenation.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/elu.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/add.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/pad.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/select.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/add_n.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/div.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/cumsum.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/exp.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/space_to_batch_nd.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/neg.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/batch_matmul.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/resize_nearest_neighbor.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/fill.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/quantize.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/mul.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/tanh.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/arg_min_max.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/portable_tensor_utils.cc
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/l2normalization.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/round.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/reduce.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/portable_tensor_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/floor_div.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/leaky_relu.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/resize_bilinear.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/pooling.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/logistic.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/mean.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/transpose_conv.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/fully_connected.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/conv.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/add.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/mul.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/tanh.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/l2normalization.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/depthwise_conv.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/integer_ops/pooling.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/maximum_minimum.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/floor.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/reference/lstm_cell.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/compatibility.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/runtime_shape.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/max.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/quantization_util.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/portable_tensor_utils.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/internal/cppmath.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/kernels/padding.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/portable_type_to_tflitetype.h
  ${CMAKE_SOURCE_DIR}/tensorflow/lite/builtin_op_data.h
)

# TODO(fabianpedd): Integrate CMSIS-NN / muRISCV-NN library
# string(REPLACE "/fully_connected.cc" "/cmsis_nn/fully_connected.cc" TFLM_SRCS ${TFLM_SRCS})
# string(REPLACE "/mul.cc"             "/cmsis_nn/mul.cc"             TFLM_SRCS ${TFLM_SRCS})
# string(REPLACE "/add.cc"             "/cmsis_nn/add.cc"             TFLM_SRCS ${TFLM_SRCS})
# string(REPLACE "/svdf.cc"            "/cmsis_nn/svdf.cc"            TFLM_SRCS ${TFLM_SRCS})
# string(REPLACE "/conv.cc"            "/cmsis_nn/conv.cc"            TFLM_SRCS ${TFLM_SRCS})
# string(REPLACE "/depthwise_conv.cc"  "/cmsis_nn/depthwise_conv.cc"  TFLM_SRCS ${TFLM_SRCS})
# string(REPLACE "/pooling.cc"         "/cmsis_nn/pooling.cc"         TFLM_SRCS ${TFLM_SRCS})
# string(REPLACE "/softmax.cc"         "/cmsis_nn/softmax.cc"         TFLM_SRCS ${TFLM_SRCS})

set(TFLM_THIRD_PARTY_SRCS
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/util.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/vector.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/flexbuffers.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/struct.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/array.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/buffer_ref.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/default_allocator.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/verifier.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/buffer.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/vector_downward.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/flatbuffers.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/table.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/stl_emulation.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/base.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/flatbuffer_builder.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/string.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/allocator.h
  ${CMAKE_SOURCE_DIR}/third_party/flatbuffers/include/flatbuffers/detached_buffer.h
  ${CMAKE_SOURCE_DIR}/third_party/gemmlowp/internal/detect_platform.h
  ${CMAKE_SOURCE_DIR}/third_party/gemmlowp/fixedpoint/fixedpoint_neon.h
  ${CMAKE_SOURCE_DIR}/third_party/gemmlowp/fixedpoint/fixedpoint_sse.h
  ${CMAKE_SOURCE_DIR}/third_party/gemmlowp/fixedpoint/fixedpoint.h
  ${CMAKE_SOURCE_DIR}/third_party/kissfft/tools/kiss_fftr.c
  ${CMAKE_SOURCE_DIR}/third_party/kissfft/tools/kiss_fftr.h
  ${CMAKE_SOURCE_DIR}/third_party/kissfft/_kiss_fft_guts.h
  ${CMAKE_SOURCE_DIR}/third_party/kissfft/kiss_fft.h
  ${CMAKE_SOURCE_DIR}/third_party/kissfft/kiss_fft.c
  ${CMAKE_SOURCE_DIR}/third_party/ruy/ruy/profiler/instrumentation.h
)

target_sources(${CMAKE_PROJECT_NAME} PRIVATE
  ${TFLM_SRCS}
  ${TFLM_THIRD_PARTY_SRCS}
)

add_subdirectory(benchmarks)
