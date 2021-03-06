load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

#=============================================================================
# Go rules

http_archive(
    name = "io_bazel_rules_go",
    urls = ["https://github.com/bazelbuild/rules_go/releases/download/0.17.0/rules_go-0.17.0.tar.gz"],
    sha256 = "492c3ac68ed9dcf527a07e6a1b2dcbf199c6bf8b35517951467ac32e421c06c1",
)

http_archive(
    name = "bazel_gazelle",
    url = "https://github.com/bazelbuild/bazel-gazelle/releases/download/0.16.0/bazel-gazelle-0.16.0.tar.gz",
    sha256 = "7949fc6cc17b5b191103e97481cf8889217263acf52e00b560683413af204fcb",
)

load("@io_bazel_rules_go//go:deps.bzl", "go_rules_dependencies", "go_register_toolchains")

go_rules_dependencies()

go_register_toolchains(
    go_version = "1.10.8",
)

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies()

#=============================================================================
# Docker rules

git_repository(
    name = "io_bazel_rules_docker",
    remote = "https://github.com/bazelbuild/rules_docker.git",
    tag = "v0.7.0",
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

container_pull(
    name = "debian_hyperkube_base_amd64",
    # 'tag' is also supported, but digest is encouraged for reproducibility.
    digest = "sha256:cc782ed16599000ca4c85d47ec6264753747ae1e77520894dca84b104a7621e2",
    registry = "k8s.gcr.io",
    repository = "debian-hyperkube-base-amd64",
    tag = "0.10",
)

git_repository(
    name = "distroless",
    remote = "https://github.com/googlecloudplatform/distroless.git",
    commit = "3585653b2b0d33c3fb369b907ef68df8344fd2ad",
)

load(
    "@distroless//package_manager:package_manager.bzl",
    "package_manager_repositories",
    "dpkg_src",
    "dpkg_list",
)

package_manager_repositories()

dpkg_src(
    name = "debian_stretch",
    arch = "amd64",
    distro = "stretch",
    sha256 = "4b981bd2445d85cf1d93ea9f2d2dc235d20543ecdadd0d8065a10793b94eab9b",
    snapshot = "20190131T155411Z",
    url = "http://snapshot.debian.org/archive",
)

dpkg_list(
    name = "package_bundle",
    packages = [
        "cgmanager",
        "dbus",
        "libapparmor1",
        "libcgmanager0",
        "libcryptsetup4",
        "libdbus-1-3",
        "libnih-dbus1",
        "libnih1",
        "libpam-systemd",
        "libprocps6",
        "libseccomp2",
        "procps",
        "systemd-shim",
        "systemd",
    ],
    sources = [
        "@debian_stretch//file:Packages.json",
    ],
)

# We use the prebuilt utils.tar.gz containing socat & conntrack, building it in bazel is really painful
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_file")

http_file(
    name = "utils_tar_gz",
    urls = ["https://kubeupv2.s3.amazonaws.com/kops/1.11.0/linux/amd64/utils.tar.gz"],
    sha256 = "7b288b4ab499186120f1f251d7a16b430073487bda8571574fcc9243925d6a30",
)
