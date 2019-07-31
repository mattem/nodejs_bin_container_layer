load("@npm_bazel_typescript//:defs.bzl", "ts_library")
load("@build_bazel_rules_nodejs//:defs.bzl", "nodejs_binary")
load("@io_bazel_rules_docker//container:container.bzl", "container_image", "container_layer")


ts_library(
    name = "app",
    srcs = glob(["*.ts"]),
    deps = [
        "//libs/a",
        "@npm//@types/node",
    ],
    tsconfig = "//:tsconfig.json",
    visibility = ['//visibility:public'],
)

nodejs_binary(
    name = "bin",
    data = [
      ":app",
    ],
    entry_point = "nodejs_bin_container_layer/app/main.js",
)

container_layer(
    name = "app_layer",
    files = [
        ":bin"
    ],
    directory = "/npm",
)

container_image(
    name = "container",
    base = "@nodejs10_13_0_base//image",
    layers = [
        ":app_layer",
    ],
    cmd = "npm/bin",
)
