include ../Makefile.inc
UPSTREAM=https://www.hiawatha-webserver.org/files/hiawatha-10.0.tar.gz

all: bin/hiawatha images

bin/hiawatha: build/build/hiawatha
	mkdir -p bin
	cp $< $@

build/build/hiawatha: build/build/Makefile
	$(MAKE) -C build/build

build/build/Makefile: build/CMakeLists.txt
	mkdir -p build/build
	(cd build/build && \
	    cmake -DCMAKE_TOOLCHAIN_FILE=$(RUMPRUN_CMAKE_TOOLCHAIN_FILE) \
	          -DENABLE_XSLT=off \
		  -Wno-dev \
		  ..)

build/CMakeLists.txt:
	mkdir -p build
	../scripts/fetch.sh $(UPSTREAM) build/hiawatha.tar.gz
	(cd build && tar -xz --strip-components 1 -f hiawatha.tar.gz)

.PHONY: images
images: images/data.iso

images/data.iso: images/data/conf/* images/data/www/*
	$(RUMPRUN_GENISOIMAGE) -o images/data.iso images/data

.PHONY: clean
clean:
	$(MAKE) -C build/build clean
	rm -f bin/*
	rm -f images/data.iso
distclean: clean
	rm -rf build/build
