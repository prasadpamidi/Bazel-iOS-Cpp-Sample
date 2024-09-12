load("@build_bazel_rules_apple//apple:ios.bzl", "ios_application", "ios_unit_test")
load("@rules_swift//swift:swift.bzl", "swift_library")
load("@rules_xcodeproj//xcodeproj:defs.bzl", "top_level_target", "xcodeproj")
load("@build_bazel_rules_apple//apple:versioning.bzl", "apple_bundle_version")
load("@rules_cc//cc:defs.bzl", "cc_library", "cc_test")

cc_library(
    name = "cpp_tests_lib",
    srcs = [
        "cpp_tests_lib/cpp_test.cpp",
    ],
    deps = [
        "@googletest//:gtest",
        "@googletest//:gtest_main",
    ],
    visibility = ["//visibility:public"],
)

objc_library(
    name = "objc_tests_lib",
    srcs = [
        "objc_tests_lib/GoogleTests.mm",
    ],
    deps = [
        ":cpp_tests_lib",
        "@googletest//:gtest",
        "@googletest//:gtest_main",
    ],
    visibility = ["//visibility:public"],
    sdk_frameworks = ["XCTest"],
)

objc_library(
    name = "objc_cpp_tests_lib",
    srcs = [
        "cpp_tests_lib/cpp_test.cpp",
        "objc_tests_lib/GoogleTests.mm",
    ],
    deps = [
        "@googletest//:gtest",
        "@googletest//:gtest_main",
    ],
    visibility = ["//visibility:public"],
    sdk_frameworks = ["XCTest"],
)

ios_unit_test(
    name = "ios_cpp_tests_lib",
    minimum_os_version = "17.0",
    deps = [":cpp_tests_lib"],
    visibility = ["//visibility:public"],
)

ios_unit_test(
    name = "ios_objc_tests_lib",
    minimum_os_version = "17.0",
    deps = [":objc_tests_lib"],
    visibility = ["//visibility:public"],
)

ios_unit_test(
    name = "ios_objc_cpp_tests_lib",
    minimum_os_version = "17.0",
    deps = [":objc_cpp_tests_lib"],
    visibility = ["//visibility:public"],
)

swift_library(
    name = "BazelSampleAppLib",
    srcs = glob(["BazelSample/*.swift"]),
    data = [
        "BazelSample/Base.lproj/LaunchScreen.storyboard",
        "BazelSample/Base.lproj/Main.storyboard",
    ],
    module_name = "BazelSampleAppLib",
    tags = ["manual"],
)

apple_bundle_version(
    name = "Version",
    build_version = "0.0.1",
    short_version_string = "1.0",
)

# Resource bundle for assets
filegroup(
    name = "Assets",
    srcs = glob(
        [
            "BazelSample/Assets.xcassets/**",
        ],
        exclude = ["BazelSample/Assets.xcassets/AppIcon.appiconset/**"],
    ),
)

ios_application(
    name = "BazelSampleApp",
    app_icons = glob(["BazelSample/Assets.xcassets/AppIcon.appiconset/**"]),
    bundle_id = "com.bazel.sample",
    families = [
        "iphone",
        "ipad",
    ],
    infoplists = ["BazelSample/Info.plist"],
    minimum_os_version = "17.0",
    resources = [
        ":Assets",
    ],
    tags = ["manual"],
    version = ":Version",
    visibility = ["//visibility:public"],
    deps = [":BazelSampleAppLib"],
)

xcodeproj(
    name = "xcodeproj",
    generation_mode = "incremental",
    project_name = "BazelSampleAppProject",
    top_level_targets = [
        top_level_target(
            ":BazelSampleApp",
            target_environments = ["simulator"],
        ),
        ":ios_cpp_tests_lib",
        ":ios_objc_tests_lib",
        ":ios_objc_cpp_tests_lib",
    ],
)