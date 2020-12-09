.PHONY: help

VERSION ?= `cat VERSION`
MAJ_VERSION := $(shell echo $(VERSION) | sed 's/\([0-9][0-9]*\)\.\([0-9][0-9]*\)\(\.[0-9][0-9]*\)*/\1/')
MIN_VERSION := $(shell echo $(VERSION) | sed 's/\([0-9][0-9]*\)\.\([0-9][0-9]*\)\(\.[0-9][0-9]*\)*/\1.\2/')
IMAGE_NAME ?= bitwalker/alpine-erlang
ARM_IMAGE_NAME ?= bitwalker/alpine-erlang-armv8

help:
	@echo "$(IMAGE_NAME):$(VERSION)"
	@perl -nle'print $& if m{^[a-zA-Z_-]+:.*?## .*$$}' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

test: ## Test the Docker image
	docker run --rm -it $(IMAGE_NAME):$(VERSION) erl -version

shell: ## Run an Erlang shell in the image
	docker run --rm -it $(IMAGE_NAME):$(VERSION) erl

sh: ## Boot to a shell prompt
	docker run --rm -it $(IMAGE_NAME):$(VERSION) /bin/bash

sh-build: ## Boot to a shell prompt in the build image
	docker run --rm -it $(IMAGE_NAME)-build:$(VERSION) /bin/bash

build: ## Build the Docker image
	docker build --squash --force-rm -t $(IMAGE_NAME):$(VERSION) -t $(IMAGE_NAME):$(MIN_VERSION) -t $(IMAGE_NAME):$(MAJ_VERSION) -t $(IMAGE_NAME):latest .

build-arm: ## Build the ARMv8 Docker image
	docker build --squash --force-rm -t $(ARM_IMAGE_NAME):$(VERSION) -t $(ARM_IMAGE_NAME):$(MIN_VERSION) -t $(ARM_IMAGE_NAME):$(MAJ_VERSION) -t $(ARM_IMAGE_NAME):latest -f Dockerfile.arm .

stage-build: ## Build the build image and stop there for debugging
	docker build --target=build -t $(IMAGE_NAME)-build:$(VERSION) .

clean: ## Clean up generated images
	@docker rmi --force $(IMAGE_NAME):$(VERSION) $(IMAGE_NAME):$(MIN_VERSION) $(IMAGE_NAME):$(MAJ_VERSION) $(IMAGE_NAME):latest
	@docker rmi --force $(ARM_IMAGE_NAME):$(VERSION) $(ARM_IMAGE_NAME):$(MIN_VERSION) $(ARM_IMAGE_NAME):$(MAJ_VERSION) $(IMAGE_NAME):latest

rebuild: clean build ## Rebuild the Docker image

release: release-x86 release-arm ## Rebuild and release the Docker image to Docker Hub

release-x86: build
	docker push $(IMAGE_NAME):$(VERSION)
	docker push $(IMAGE_NAME):latest

release-arm: build-arm
	docker push $(ARM_IMAGE_NAME):$(VERSION)
	docker push $(ARM_IMAGE_NAME):latest
