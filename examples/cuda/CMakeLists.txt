# -----------------------------------------------------------------------------
# cuda examples
# -----------------------------------------------------------------------------

list(APPEND TF_CUDA_EXAMPLES 
  cuda_device_property 
  cuda_saxpy 
  cuda_matmul 
  cuda_knn
  cuda_capturer
  cuda_reduce
  cuda_transform
  cuda_rebind
)

foreach(cuda_example IN LISTS TF_CUDA_EXAMPLES)
  add_executable(${cuda_example} ${cuda_example}.cu)
  target_link_libraries(${cuda_example}
    ${PROJECT_NAME} Threads::Threads tf::default_settings
  )

  # avoid cmake 3.18+ warning
  # we let nvcc to decide the flag if the architecture is not given
  if(NOT CUDA_ARCHITECTURES)
    set_property(TARGET ${cuda_example} PROPERTY CUDA_ARCHITECTURES OFF)
  endif()
endforeach()

# -----------------------------------------------------------------------------
# CUDA BLAS examples
# -----------------------------------------------------------------------------

if(CUBLAS_FOUND)
add_subdirectory(cublas)
endif()

