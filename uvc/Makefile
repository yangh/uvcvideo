KDIR ?= /lib/modules/`uname -r`/build

default:
	$(MAKE) -C $(KDIR) M=$$PWD
	sudo sh test-uvc

# Module specific targets
clean:
	make -C $(KDIR) M=$$PWD clean

