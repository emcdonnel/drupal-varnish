-include env_make

VARNISH_VER ?= 4.1.3
TAG ?= 4.1

REPO = wodby/drupal-varnish
NAME = drupal-varnish-$(VARNISH_VER)
FROM_TAG = $(VARNISH_VER)

ifneq ($(FROM_STABILITY_TAG),)
    FROM_TAG := $(FROM_TAG)-$(FROM_STABILITY_TAG)
endif

.PHONY: build test test-clean push shell run start stop logs clean release

default: build

build:
	docker build -t $(REPO):$(TAG) --build-arg FROM_TAG=$(FROM_TAG) ./

test:
	cd ./test && IMAGE=$(REPO):$(TAG) ./test.sh

push:
	docker push $(REPO):$(TAG)

shell:
	docker run --rm --name $(NAME) -i -t $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(TAG) /bin/bash

run:
	docker run --rm --name $(NAME) $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(TAG) $(CMD)

start:
	docker run -d --name $(NAME) $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(TAG)

stop:
	docker stop $(NAME)

logs:
	docker logs $(NAME)

clean:
	-docker rm -f $(NAME)

release: build push