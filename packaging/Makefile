default:
	echo "try make image-[amd64|arm64], make push-[amd64|arm64], make manifest, or make push-manifest"

image-amd64:
	DOCKER_BUILDKIT=1 docker build -t gristlabs/gvisor-unprivileged:buster-amd64 .

push-amd64:
	docker push gristlabs/gvisor-unprivileged:buster-amd64

image-arm64:
	DOCKER_BUILDKIT=1 docker build -t gristlabs/gvisor-unprivileged:buster-arm64 .

push-arm64:
	docker push gristlabs/gvisor-unprivileged:buster-arm64

manifest:
	docker manifest rm gristlabs/gvisor-unprivileged:buster
	docker manifest create gristlabs/gvisor-unprivileged:buster \
	gristlabs/gvisor-unprivileged:buster-amd64 \
	gristlabs/gvisor-unprivileged:buster-arm64

push-manifest:
	docker manifest push gristlabs/gvisor-unprivileged:buster
	echo "remember to delete the temporary images from Docker Hub"
