STANDALONE     = 1

include ../../User/Makefile

.DEFAULT_GOAL := all
PRODUCT        = disklabel$(SUFFIX)

OBJS          += disklabel.o

ifeq ($(DIST),Darwin)
	LDFLAGS += -Wl,-framework,CoreFoundation -Wl,-framework,CoreGraphics -Wl,-framework,CoreText
endif

all: $(PRODUCT)

$(PRODUCT): $(OBJS)
	$(CC) $(LDFLAGS) $(OBJS) -o $(PRODUCT)
	$(STRIP) $(PRODUCT)

clean:
	rm -rf *.o $(PRODUCT)
