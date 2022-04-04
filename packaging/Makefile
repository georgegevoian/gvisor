IMAGE_NAME := gristlabs/gvisor-unprivileged:buster

## usage: make <image-amd64|image-arm64|push-amd64|push-arm64|manifest|push-manifest>
##
## Basic targets.
##
##   This Makefile wraps Docker commands for building and pushing multi-arch gvisor
##   images. As of this writing, multi-arch builds via QEMU have a conflict with
##   Bazel, so we have to build individual images for each architecture. The general
##   flow is:
##
##   1. Build and push images for amd64 and arm64 architectures, using the appropriate
##   image and push targets. The host must match the architecture of the target, so 2
##   different machines must be used: one for the amd64 build, and one for the arm64 build.
##   2. Once both images are published on Docker Hub, run the manifest target. This will
##   prepare the manifest for the final combined multi-arch image.
##   3. Finally, run the push-manifest target. This will update the main multi-arch image
##   to contain references to the architecture-specific images built in step 1.
##
##   Afterwards, the temporary images created in step 1 may be removed from Docker Hub
##   via the web interface.
##
help:
	@echo "usage: make [target]"
	@echo "targets: image-amd64, image-arm64, push-amd64, push-arm64, manifest, push-manifest"

default:
	help

image-amd64:
	DOCKER_BUILDKIT=1 docker build -t $(IMAGE_NAME)-amd64 .

push-amd64:
	docker push $(IMAGE_NAME)-amd64

image-arm64:
	DOCKER_BUILDKIT=1 docker build -t $(IMAGE_NAME)-arm64 .

push-arm64:
	docker push $(IMAGE_NAME)-arm64

manifest:
	docker manifest rm $(IMAGE_NAME)
	docker manifest create $(IMAGE_NAME) \
	$(IMAGE_NAME)-amd64 \
	$(IMAGE_NAME)-arm64

push-manifest:
	docker manifest push $(IMAGE_NAME)
	@echo "remember to delete the temporary images from Docker Hub"
