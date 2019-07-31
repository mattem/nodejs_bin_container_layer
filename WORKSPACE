workspace(name = "nodejs_bin_container_layer")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive", "http_file")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

skylib_version = "0.8.0"
http_archive(
    name = "bazel_skylib",
    type = "tar.gz",
    url = "https://github.com/bazelbuild/bazel-skylib/releases/download/{}/bazel-skylib.{}.tar.gz".format (skylib_version, skylib_version),
    sha256 = "2ef429f5d7ce7111263289644d233707dba35e39696377ebab8b0bc701f7818e",
)

git_repository(
    name = "build_bazel_rules_nodejs",
    # version 0.27.10
    commit = "3a732cd56900597b90a9557a49f90cd544440115",
    remote = "https://github.com/bazelbuild/rules_nodejs.git",
)

load("@build_bazel_rules_nodejs//:defs.bzl", "node_repositories", "yarn_install")

node_repositories(
    node_repositories = {
        "10.15.3-darwin_amd64": ("node-v10.15.3-darwin-x64.tar.gz", "node-v10.15.3-darwin-x64", "7a5eaa1f69614375a695ccb62017248e5dcc15b0b8edffa7db5b52997cf992ba"),
        "10.15.3-linux_amd64": ("node-v10.15.3-linux-x64.tar.xz", "node-v10.15.3-linux-x64", "faddbe418064baf2226c2fcbd038c3ef4ae6f936eb952a1138c7ff8cfe862438"),
        "10.15.3-windows_amd64": ("node-v10.15.3-win-x64.zip", "node-v10.15.3-win-x64", "93c881fdc0455a932dd5b506a7a03df27d9fe36155c1d3f351ebfa4e20bf1c0d"),
    },
    node_urls = ["https://nodejs.org/dist/v{version}/{filename}"],
    node_version = "10.15.3",
    yarn_version = "1.12.1",
)

yarn_install(
    name = "npm",
    package_json = "//:package.json",
    yarn_lock = "//:yarn.lock",
)

load("@npm//:install_bazel_dependencies.bzl", "install_bazel_dependencies")
install_bazel_dependencies()

load("@npm_bazel_typescript//:defs.bzl", "ts_setup_workspace")
ts_setup_workspace()

git_repository(
    name = "io_bazel_rules_docker",
    remote = "https://github.com/bazelbuild/rules_docker.git",
    commit = "77718a2a0b7ac8573a5865cc18f3f355d48f9048",
)

http_archive(
    name = "io_bazel_rules_go",
    urls = [
        "https://storage.googleapis.com/bazel-mirror/github.com/bazelbuild/rules_go/releases/download/0.18.6/rules_go-0.18.6.tar.gz",
        "https://github.com/bazelbuild/rules_go/releases/download/0.18.6/rules_go-0.18.6.tar.gz",
    ],
    sha256 = "f04d2373bcaf8aa09bccb08a98a57e721306c8f6043a2a0ee610fd6853dcde3d",
)

load("@io_bazel_rules_go//go:deps.bzl", "go_rules_dependencies", "go_register_toolchains")
go_rules_dependencies()
go_register_toolchains()

load("@io_bazel_rules_docker//repositories:repositories.bzl", container_repositories = "repositories")
container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")
container_deps()

load("@io_bazel_rules_docker//container:container.bzl", "container_pull")
container_pull(
    name = "nodejs10_13_0_base",
    registry = "index.docker.io",
    repository = "library/node",
    tag = "10.13.0-slim",
)

