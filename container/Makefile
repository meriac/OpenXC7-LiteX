.PHONY: run rebuild build clean login publish status version multi

CONTAINER_BASEIMG:=ghcr.io/meriac/openxc7-dev:latest
CONTAINER_REGISTRY:=ghcr.io
CONTAINER_OWNER:=meriac
CONTAINER_NAME:=openxc7-litex
CONTAINER_PREFIX:=$(CONTAINER_REGISTRY)/$(CONTAINER_OWNER)/$(CONTAINER_NAME)

# determine container architecture
CONTAINER_ARCH:=$(shell uname -m)
ifeq ("$(CONTAINER_ARCH)","x86_64")
	CONTAINER_ARCH:=amd64
endif

CONTAINER:=$(CONTAINER_PREFIX):$(CONTAINER_ARCH)
CONTAINER_PLATFORM:=linux/$(CONTAINER_ARCH)

# If SELinux is enabled, add ':Z' to mapped volumes
SELINUX_SECURITY:=$(shell if [ "$$(getenforce 2>&1)" = "Enforcing" ]; then echo ":Z"; else echo ""; fi)

run:
	podman run -h $(CONTAINER_NAME) --rm -v $(PWD)/workspaces:/workspaces$(SELINUX_SECURITY) --platform $(CONTAINER_PLATFORM) -it $(CONTAINER)

rebuild: clean build run

build:
	# ensure to have a fresh base image on every build
	podman rmi -f $(CONTAINER_BASEIMG)
	# build depending image
	podman build -t $(CONTAINER) -f Containerfile --platform $(CONTAINER_PLATFORM) --build-arg BASEIMG=$(CONTAINER_BASEIMG)

clean:
	podman rmi -f $(CONTAINER)

login:
	@echo $(CR_PAT) | podman login $(CONTAINER_REGISTRY) -u DOESNT_MATTER --password-stdin

publish: status
	podman push --compress --compression-level 9 $(CONTAINER)

status:
	podman image tree $(CONTAINER)

version:
	@echo $(CONTAINER)

manifest:
	podman rmi -f $(CONTAINER_PREFIX):latest
	podman manifest create $(CONTAINER_PREFIX):latest $(CONTAINER_PREFIX):amd64 $(CONTAINER_PREFIX):arm64
	podman push --compress --compression-level 9 $(CONTAINER_PREFIX):latest
	podman rmi -f $(CONTAINER_PREFIX):latest
