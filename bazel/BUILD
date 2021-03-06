# envoy_cmake_external(
#     name = "event",
#     cache_entries = {
#         "EVENT__DISABLE_OPENSSL": "on",
#         "EVENT__DISABLE_REGRESS": "on",
#         "EVENT__DISABLE_TESTS": "on",
#         "EVENT__LIBRARY_TYPE": "STATIC",
#         # Force _GNU_SOURCE on for Android builds. This would be contained in
#         # a 'select' but the downstream macro uses a select on all of these
#         # options, and they cannot be nested.
#         # If https://github.com/bazelbuild/rules_foreign_cc/issues/289 is fixed
#         # this can be removed.
#         # More details https://github.com/lyft/envoy-mobile/issues/116
#         "_GNU_SOURCE": "on",
#     },
#     lib_source = "@com_github_libevent_libevent//:all",
#     static_libraries = select({
#         # macOS organization of libevent is different from Windows/Linux.
#         # Including libevent_core is a requirement on those platforms, but
#         # results in duplicate symbols when built on macOS.
#         # See https://github.com/lyft/envoy-mobile/issues/677 for details.
#         "//bazel:apple": [
#             "libevent.a",
#             "libevent_pthreads.a",
#         ],
#         "//bazel:windows_x86_64": [
#             "event.lib",
#             "event_core.lib",
#         ],
#         "//conditions:default": [
#             "libevent.a",
#             "libevent_pthreads.a",
#             "libevent_core.a",
#         ],
#     }),
# )
package(default_visibility = ["//visibility:public"])

load("@rules_foreign_cc//tools/build_defs:configure.bzl", "configure_make")
load("@rules_foreign_cc//tools/build_defs:cmake.bzl", "cmake_external")
configure_make(
    name = "event",
    configure_options = [
        "--enable-shared=no",
        "--disable-libevent-regress",
        "--disable-openssl",
    ],
    lib_source = "@libevent//:all",
    out_lib_dir = "lib",
    # libevent script uses it's own libtool for linking;
    # so do not specify linker tool for it
    # (otherwise, if the libtool from bazel's toolchain is supplied,
    # the build script has problems with passing output file to libtool)
    # see #315
    configure_env_vars = {
        "AR": "",
    },
)
config_setting(
    name = "darwin",
    values = {"cpu": "darwin"},
)

cmake_external(
    name = "libevent",
    cache_entries = {
        "EVENT__DISABLE_OPENSSL": "on",
         "EVENT__DISABLE_REGRESS": "on",
         "EVENT__DISABLE_TESTS": "on",
         "EVENT__LIBRARY_TYPE": "STATIC",
    },
    cmake_options = ["-GNinja"],
    make_commands = ["ninja -v", "ninja -v install"],
    # headers_only = True,
    lib_source = "@com_github_libevent//:all",
    out_lib_dir = "lib",
    static_libraries = [
             "libevent.a",
             "libevent_pthreads.a",
         ]

)