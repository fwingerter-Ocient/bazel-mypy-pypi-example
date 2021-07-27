load("@mypy_integration//:mypy.bzl", "mypy_test")
load("@mypy_integration//:rules.bzl", "mypy_stubs")
load("@mypy_stubs//:requirements.bzl", "requirement")

# this binary needs type stubs from pypi. we add them to deps, but it has no effect on subsequent mypy_test targets.
py_binary(
    name = "uses-deps",
    srcs = ["uses-deps.py"],
    deps = [
        requirement("types_python_dateutil"),
    ],
)

# we can define a mypy_stubs target manually pointing at the .pyi files, but mypy doesn't use them
mypy_stubs(
    name = "stubs-dateutil-pyi-files",
    srcs = [
        requirement("types_python_dateutil") + ":dateutil-stubs/__init__.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/_common.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/easter.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/parser.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/relativedelta.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/rrule.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/tz/__init__.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/tz/_common.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/tz/tz.pyi",
        requirement("types_python_dateutil") + ":dateutil-stubs/utils.pyi",
    ],
)

# this approach (directly referencing the dep from pypi via `requirement`) causes a bazel error if used as a dep of mypy_test, see below
mypy_stubs(
    name = "stubs-dateutil-requirement",
    srcs = [
        requirement("types_python_dateutil"),
    ],
)

mypy_test(
    name = "uses_deps_mypy",
    deps = [":uses-deps"] + [
        requirement("types_python_dateutil"),  # this doesn't avoid 'Library stubs not installed for "dateutil"'
        ":stubs-dateutil-pyi-files",  # nor does this
        #":stubs-dateutil-requirement",
        # uncommenting the above results in:
        # ERROR: bazel-mypy-pypi-example/BUILD:32:11: in srcs attribute of mypy_stubs rule //:stubs-dateutil-doesnt-work: '@mypy_stubs//pypi__types_python_dateutil:pypi__types_python_dateutil' does not produce any mypy_stubs srcs files (expected .pyi)
    ],
)
