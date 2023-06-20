workspace(name = "tf_serving")

# ===== TensorFlow dependency =====
#
# TensorFlow is imported here instead of in tf_serving_workspace() because
# existing automation scripts that bump the TF commit hash expect it here.
#
# To update TensorFlow to a new revision.
# 1. Update the 'git_commit' args below to include the new git hash.
# 2. Get the sha256 hash of the archive with a command such as...
#    curl -L https://github.com/tensorflow/tensorflow/archive/<git hash>.tar.gz | sha256sum
#    and update the 'sha256' arg with the result.
# 3. Request the new archive to be mirrored on mirror.bazel.build for more
#    reliable downloads.
load("//tensorflow_serving:repo.bzl", "tensorflow_http_archive")
tensorflow_http_archive(
    name = "org_tensorflow",
    sha256 = "2f44f6b57e065a545d16a64a428d91ec2006def421a6ea44940e159048f15f0c",
    git_commit = "1b8f5c396f0c016ebe81fe1af029e6f205c926a4",
)

# Import all of TensorFlow Serving's external dependencies.
# Downstream projects (projects importing TensorFlow Serving) need to
# duplicate all code below in their WORKSPACE file in order to also initialize
# those external dependencies.
load("//tensorflow_serving:workspace.bzl", "tf_serving_workspace")
tf_serving_workspace()

# Check bazel version requirement, which is stricter than TensorFlow's.
load(
    "@org_tensorflow//tensorflow:version_check.bzl",
    "check_bazel_version_at_least"
)
check_bazel_version_at_least("3.7.2")

# Initialize TensorFlow's external dependencies.
load("@org_tensorflow//tensorflow:workspace3.bzl", "workspace")
workspace()
load("@org_tensorflow//tensorflow:workspace2.bzl", "workspace")
workspace()
load("@org_tensorflow//tensorflow:workspace1.bzl", "workspace")
workspace()
load("@org_tensorflow//tensorflow:workspace0.bzl", "workspace")
workspace()

# Initialize bazel package rules' external dependencies.
load("@rules_pkg//:deps.bzl", "rules_pkg_dependencies")
rules_pkg_dependencies()

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")

rules_proto_dependencies()
rules_proto_toolchains()

# START recommenders_addons
load("//build_deps/tf_dependency:tf_configure.bzl", "tf_configure")
load("//build_deps/toolchains/gpu:cuda_configure.bzl", "cuda_configure")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "cub_archive",
    build_file = "//build_deps/toolchains/gpu:cub.BUILD",
    sha256 = "6bfa06ab52a650ae7ee6963143a0bbc667d6504822cbd9670369b598f18c58c3",
    strip_prefix = "cub-1.8.0",
    urls = [
        "https://storage.googleapis.com/mirror.tensorflow.org/github.com/NVlabs/cub/archive/1.8.0.zip",
        "https://github.com/NVlabs/cub/archive/1.8.0.zip",
    ],
)

tf_configure(
    name = "local_config_tf",
)

# cuda_configure(name = "local_config_cuda")

# END recommenders_addons

