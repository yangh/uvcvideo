all: vc test

vc: capture.c
	gcc -g -Wall -pthread -o $@ $<

test:
	sh test-vc

clean:
	rm -f vc
