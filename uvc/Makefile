obj-m := uvcvideo.o

uvcvideo-objs  := uvc_driver.o uvc_queue.o uvc_v4l2.o uvc_video.o uvc_ctrl.o \
		  uvc_status.o uvc_isight.o

KVERSION := /lib/modules/$(shell uname -r)/build

all: kmod

kmod:
	make -C $(KVERSION) M=$(PWD) modules

test:
	sudo ./test-uvc

clean:
	make -C $(KVERSION) M=$(PWD) clean

