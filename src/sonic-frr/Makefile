.ONESHELL:
SHELL = /bin/bash
.SHELLFLAGS += -e

MAIN_TARGET = frr_$(FRR_VERSION)-1~sonic.debian8+1_amd64.deb

$(addprefix $(DEST)/, $(MAIN_TARGET)): $(DEST)/% :

	# Build the package
	pushd ./frr

	# clean up the previous build
	rm -rf debian
	rm frr*.tar.gz
	rm frr*.tar.xz
	rm frr*.dsc

	# make a dist tarball
	./bootstrap.sh
	./configure
	make dist

	# Create backports debian sources
	cp -a debianpkg debian
	make -f debian/rules backports

	# new directory to build the package 
	rm -rf frrpkg
	mkdir frrpkg
	cd frrpkg
	tar xf ../frr_*.orig.tar.gz
	cd frr*
	tar xf ../../frr_*sonic.debian8*.debian.tar.xz

	# build package
	dpkg-buildpackage -rfakeroot -b -us -uc
	cd ..
	mv $* $(DEST)/

	popd

