default:
	echo "try make image or make push"

image:
	DOCKER_BUILDKIT=1 docker build -t gristlabs/gvisor-unprivileged:buster .

push:
	docker push gristlabs/gvisor-unprivileged:buster
