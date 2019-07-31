load("@npm_bazel_typescript//:defs.bzl", "ts_library")

ts_library(
    name = "a",
    srcs = glob(["*.ts"]),
    deps = [
        "@npm//@types/node"
    ],
    module_name = "@lib/a",
    tsconfig = "//:tsconfig.json",
    visibility = ['//visibility:public'],
)
