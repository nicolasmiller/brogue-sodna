CC = gcc
CFLAGS = -Iinclude -Wall -Werror -O3 -lm
LDFLAGS =
AR = ar

all: libsodna_sdl2.so libsodna_sdl2.a demo

sodna_sdl2.o: include/sodna.h src_sdl2/sodna_sdl2.c src_sdl2/font8.inc
	$(CC) $(CFLAGS) -fPIC $$(sdl2-config --cflags) -c src_sdl2/sodna_sdl2.c -o $@

# Prebuilt the font data include for simplicity. The steps below are how
# you'd manually generate it if you have xxd and ImageMagic available.
#
#src_sdl2/font8.inc: src_sdl2/font8.gray
#	cat $< | xxd -i > $@
#
#src_sdl2/font8.gray: src_sdl2/font8.png
#	convert $< $@

libsodna_sdl2.so: sodna_sdl2.o
	$(CC) $(LDFLAGS) $< -shared -o $@ 

libsodna_sdl2.a: sodna_sdl2.o
	$(AR) $(LDFLAGS) rcs $@ $< 

demo: libsodna_sdl2.so src_demo/demo.c sodna_util/sodna_util.c
	$(CC) $(CFLAGS) $(LDFLAGS) -Isodna_util -L. -lsodna_sdl2 -Wl,-rpath=. -o $@ `sdl2-config --libs` src_demo/demo.c sodna_util/sodna_util.c

clean:
	rm -f *.o *.so *.a demo
