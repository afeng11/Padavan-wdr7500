SRC_NAME=json-c-json-c-0.17-20230812
SRC_URL=https://github.com/json-c/json-c/archive/refs/tags/json-c-0.17-20230812.tar.gz

all: download_test extract_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)/build

download_test:
	( if [ ! -f $(SRC_NAME).tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar zxf $(SRC_NAME).tar.gz; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	mkdir build; \
	cd build; \
	../cmake-configure \
		--prefix=$(STAGEDIR) \
		--enable-static \
		--enable-shared ; \
	)

clean:
	( rm -f $(SRC_NAME) ; \
	rm -f config_done ; \
	)

install:
	$(MAKE) -C $(SRC_NAME)/build install DESTDIR=""

romfs:
	cp -fP $(SRC_NAME)/build/libjson-c.so* $(ROMFSDIR)/lib
