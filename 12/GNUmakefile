CFGT?=`go env GOPATH`/bin/cfgt
EXTRA_OPTS ?=
PACKER?=`go env GOPATH`/bin/packer
TARGET ?= vmware-iso

default: help

# Image Targets.  Sort newest images first.

12.0-20170602-zfs: 12.0-amd64-20170602-zfs.json ## Build ZFS root FreeBSD-12.0-CURRENT-amd64-20170602-r319481
	@exec $(MAKE) _build TARGET=$(TARGET) EXTRA_OPTS="$(EXTRA_OPTS)" TEMPLATE=$<

12.0-20170526-zfs: 12.0-amd64-20170526-zfs.json ## Build ZFS root FreeBSD-12.0-CURRENT-amd64-20170526-r318945
	@exec $(MAKE) _build TARGET=$(TARGET) EXTRA_OPTS="$(EXTRA_OPTS)" TEMPLATE=$<


# Misc Targets

images:: ## Display the list of available images
	@grep -E '^[0-9.]+-[a-zA-Z0-9_.-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort -r | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-15s\033[0m %s\n", $$1, $$2}'

_build:
	@if [ -z "$(TARGET)" ]; then \
		printf "ERROR: specify a packer target. HINT: %s TARGET=<NAME>\n" "`basename $(MAKE)`"; \
		exit 1; \
	fi
	@if [ -z "$(TEMPLATE)" ]; then \
		printf "ERROR: specify a packer template. HINT: %s TEMPLATE=<NAME>\n" "`basename $(MAKE)`"; \
		exit 1; \
	fi
	$(PACKER) build -only=$(TARGET) -var boot_wait=35s $(EXTRA_OPTS) $(TEMPLATE)

.SUFFIXES: .json .json5
%.json: %.json5
	$(CFGT) -i $< -o $@

.PHONY: help
help::
	@echo "Image Build Targets:"
	@$(MAKE) images
	@echo
	@echo "Misc targets:"
	@grep -E '^[a-zA-Z_.-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-15s\033[0m %s\n", $$1, $$2}'
