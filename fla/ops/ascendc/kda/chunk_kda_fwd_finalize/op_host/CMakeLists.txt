add_op_to_compiled_list()

if(BUILD_OPEN_PROJECT)
    target_sources(op_host_aclnnExc PRIVATE
        chunk_kda_fwd_finalize_def.cpp
    )
endif()

add_modules_sources(OPTYPE chunk_kda_fwd_finalize ACLNNTYPE aclnn_exclude)
add_ops_compile_options(
    OP_NAME ChunkKdaFwdFinalize
    OPTIONS --cce-auto-sync=off
            -Wno-deprecated-declarations
)
if("${ASCEND_COMPUTE_UNIT}" STREQUAL "ascend950")
    add_ops_compile_options(
        OP_NAME ChunkKdaFwdFinalize
        COMPUTE_UNIT Ascend950PR_9599
        OPTIONS -DENABLE_CV_COMM_VIA_SSBUF=true
    )
endif()
# A6（Ascend960DT）复用 arch35，CV 通信宏同 A5；另按 ops-nn#11536 关闭 dcache flush
if("${ASCEND_COMPUTE_UNIT}" STREQUAL "ascend960dt")
    add_ops_compile_options(
        OP_NAME ChunkKdaFwdFinalize
        COMPUTE_UNIT Ascend960DT_968B7
        OPTIONS -DENABLE_CV_COMM_VIA_SSBUF=true
                --cce-no-dcache-flush
    )
endif()
