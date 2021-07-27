workspace(name = "bazel-mypy-pypi-example")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

rules_python_version = "c361e789a246c81649dc8d9700d29b8a32a3d00f"  # jonathon--issue-381-may2021

http_archive(
    name = "rules_python",
    sha256 = "a6f339763303be086c7c51a38bc214c020d15709b0b82e4b1bedcf4a31e9bd6a",
    strip_prefix = "rules_python-{version}".format(version = rules_python_version),
    url = "https://github.com/bazelbuild/rules_python/archive/{version}.tar.gz".format(version = rules_python_version),
)

load("@rules_python//python:pip.bzl", "pip_install")

pip_install(
    name = "mypy_stubs",
    requirements = "//mypy_stubs:requirements.txt",
)

mypy_integration_version = "0.2.0"  # Latest @ 26th June 2021

http_archive(
    name = "mypy_integration",
    sha256 = "621df076709dc72809add1f5fe187b213fee5f9b92e39eb33851ab13487bd67d",
    strip_prefix = "bazel-mypy-integration-{version}".format(version = mypy_integration_version),
    urls = [
        "https://github.com/thundergolfer/bazel-mypy-integration/archive/refs/tags/{version}.tar.gz".format(version = mypy_integration_version),
    ],
)

load(
    "@mypy_integration//repositories:repositories.bzl",
    mypy_integration_repositories = "repositories",
)

mypy_integration_repositories()

# Optionally pass a MyPy config file, otherwise pass no argument.
load("@mypy_integration//:config.bzl", "mypy_configuration")

mypy_configuration()

load("@mypy_integration//repositories:deps.bzl", mypy_integration_deps = "deps")

mypy_integration_deps(
    mypy_requirements_file = "//:mypy_version.txt",
)
