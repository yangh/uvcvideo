all: vc test

vc: capture.c
	gcc -Wall -o $@ $<

test:
	./test-vc

clean:
	rm -f vc
