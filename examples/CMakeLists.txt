link_libraries(${GFLAGS_LIBRARIES})
include_directories(${GFLAGS_INCLUDE_DIRS})

add_executable(simple-example simple/simple.cpp)
target_link_libraries(simple-example PRIVATE photon_static)

add_executable(net-perf perf/net-perf.cpp)
target_link_libraries(net-perf PRIVATE photon_static)
if (PHOTON_ENABLE_RSOCKET)
    target_compile_definitions(net-perf PRIVATE PHOTON_ENABLE_RSOCKET=1)
endif ()
if (PHOTON_ENABLE_URING)
    target_compile_definitions(net-perf PRIVATE PHOTON_URING=1)
endif()

add_executable(workpool-perf perf/workpool-perf.cpp)
target_link_libraries(workpool-perf PRIVATE photon_static)

if (NOT APPLE)
    add_executable(io-perf perf/io-perf.cpp)
    target_link_libraries(io-perf PRIVATE photon_static)

    add_executable(multi-conn-perf perf/multi-conn-perf.cpp)
    target_link_libraries(multi-conn-perf PRIVATE photon_static)
endif ()

add_executable(rpc-example-client rpc/client.cpp rpc/client_main.cpp)
target_link_libraries(rpc-example-client PRIVATE photon_static)

add_executable(rpc-example-server rpc/server.cpp rpc/server_main.cpp)
target_link_libraries(rpc-example-server PRIVATE photon_static)

add_executable(sync-primitive sync-primitive/sync-primitive.cpp)
target_link_libraries(sync-primitive PRIVATE photon_static)

add_executable(vdma-example vdma-transfer/vdma-example.cpp)
target_link_libraries(vdma-example PRIVATE photon_static)

if (PHOTON_ENABLE_FSTACK_DPDK)
    add_executable(fstack-dpdk-demo fstack-dpdk/fstack-dpdk-demo.cpp)
    target_link_libraries(fstack-dpdk-demo PRIVATE ${DPDK_LIBRARIES} ${FSTACK_LIBRARIES} photon_static)
endif ()

if (PHOTON_ENABLE_SPDK)
    add_executable(bdev-example spdk/bdev_read_write.cpp)
    target_link_libraries(bdev-example PRIVATE ${DPDK_LIBRARIES} ${SPDK_LIBRARIES} photon_static)
    add_executable(nvme-example spdk/nvme_read_write.cpp)
    target_link_libraries(nvme-example PRIVATE ${DPDK_LIBRARIES} ${SPDK_LIBRARIES} photon_static)
    add_executable(bdev_photon_server spdk/bdev_module/bdev_photon_server_main.cpp spdk/bdev_module/bdev_photon_server.cpp)
    target_link_libraries(bdev_photon_server PRIVATE ${DPDK_LIBRARIES} ${SPDK_LIBRARIES} photon_static)
endif ()