add_custom_target(CYAN_RUN_TESTS
                  COMMAND "${CMAKE_CTEST_COMMAND}"
                  --output-on-failure
                  -C "$<CONFIG>"
                  -R "^cyan_test_"
                  WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
                  USES_TERMINAL
                  VERBATIM
                  )

function(cyan_test
         TEST_SOURCE
         )
    cmake_path(ABSOLUTE_PATH TEST_SOURCE
               BASE_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
               NORMALIZE
               OUTPUT_VARIABLE TEST_SOURCE_ABS)
    file(RELATIVE_PATH REL_PATH
         "${CMAKE_CURRENT_SOURCE_DIR}" "${TEST_SOURCE_ABS}")    
    string(MAKE_C_IDENTIFIER "${REL_PATH}"
           TARGET_SUFFIX)
    set(TARGET_NAME "cyan_test_${TARGET_SUFFIX}")

    add_executable("${TARGET_NAME}" "${TEST_SOURCE_ABS}" ${ARGN})
    target_link_libraries("${TARGET_NAME}" PRIVATE Cyan::Cyan)

    add_test(NAME "${TARGET_NAME}"
             COMMAND "${TARGET_NAME}")
    add_dependencies(CYAN_RUN_TESTS "${TARGET_NAME}")

endfunction()