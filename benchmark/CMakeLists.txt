function(
    cyan_benchmark
    BENCH_SOURCE
)
    cmake_path(
        ABSOLUTE_PATH BENCH_SOURCE
        BASE_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
        NORMALIZE
        OUTPUT_VARIABLE BENCH_SOURCE_ABS
    )
    file(
        RELATIVE_PATH REL_PATH "${CMAKE_CURRENT_SOURCE_DIR}"
        "${BENCH_SOURCE_ABS}"
    )    
    string(MAKE_C_IDENTIFIER "${REL_PATH}" TARGET_SUFFIX)
    set(TARGET_NAME "cyan_bench_${TARGET_SUFFIX}")
    set_property(
        GLOBAL APPEND
        PROPERTY cyan_bench_commands_global
        COMMAND "$<TARGET_FILE:${TARGET_NAME}>"
    )

    add_executable(${TARGET_NAME} "${BENCH_SOURCE_ABS}" ${ARGN})
    target_link_libraries(${TARGET_NAME} PRIVATE
        Cyan::Cyan
    )
    target_compile_definitions(${TARGET_NAME} PRIVATE
        $<$<CONFIG:RelWithDebInfo>
            CYAN_BENCH_RDI=
        >
    )
    
    if(MSVC)
        target_compile_options(${TARGET_NAME} PRIVATE
            /nologo
            /volatile:iso
            /MP
            /WX
            /W4
            /EHs-c-
            /GR-
            $<$<CONFIG:Debug>:
                /Od
                /Zi
                /Zf
                /ZH:SHA512
            >
            $<$<CONFIG:RelWithDebInfo>:
                /Oi
                /O2
                /GA
                /GF
                /Gw
                /Gy
                /jumptablerdata
                /Zi
                /Zf
                /ZH:SHA512
            >
            $<$<CONFIG:Release>:
                /Oi
                /GL
                /Ox
                /GA
                /GF
                /Gw
                /Gy
                /jumptablerdata
            >
        )
        target_link_options(${TARGET_NAME} PRIVATE
            /nologo
            $<$<CONFIG:Debug>:
                /DEBUG
            >
            $<$<CONFIG:RelWithDebInfo>:
                /DEBUG
                /OPT:REF,ICF,LBR
            >
            $<$<CONFIG:Release>:
                /LTCG
                /OPT:REF,ICF,LBR
            >
        )
    else()
        target_compile_options(${TARGET_NAME} PRIVATE
            -nologo
            -Werror
            -Wall
            -Wextra
            -Wpedantic
            -Wshadow
            -Wconversion
            -Wsign-conversion
            -fno-rtti
            -fno-exceptions
            $<$<CONFIG:Debug>:
                -Og
            >
            $<$<CONFIG:RelWithDebInfo>:
                -O2
            >
            $<$<CONFIG:Release>:
                -fno-ident
                -O3
            >
        )
        target_link_options(${TARGET_NAME} PRIVATE
            $<$<CONFIG:Debug>:
                -g
            >
            $<$<CONFIG:RelWithDebInfo>:
                -g
            >
        )
        if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
            target_compile_options(${TARGET_NAME} PRIVATE
                $<$<CONFIG:Debug>:
                    -g3
                    -ggdb3
                    -gdwarf-5
                >
                $<$<CONFIG:RelWithDebInfo>:
                    -g3
                    -ggdb3
                    -gdwarf-5
                >
            )
            target_link_options(${TARGET_NAME} PRIVATE
                $<$<CONFIG:Release>:
                    -flto=auto
                    LINKER:--gc-sections
                >
            )
        else()
            target_compile_options(${TARGET_NAME} PRIVATE
                $<$<CONFIG:Debug>:
                    -g
                    -fdebug-default-version=5
                >
                $<$<CONFIG:RelWithDebInfo>:
                    -g
                    -fdebug-default-version=5
                >
            )
            target_link_options(${TARGET_NAME} PRIVATE
                $<$<CONFIG:Release>:
                    -flto=full
                    -dead-strip
                >
            )
        endif()
    endif()

endfunction()

# the benchmarks


get_property(cyan_bench_commands GLOBAL PROPERTY cyan_bench_commands_global)
if(cyan_bench_commands)
    add_custom_target(
        CYAN_RUN_BENCHMARKS
        ${cyan_bench_commands}
        WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
        USES_TERMINAL
        VERBATIM
    )
else()
    add_custom_target(
        CYAN_RUN_BENCHMARKS
        WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
        VERBATIM
    )
endif()