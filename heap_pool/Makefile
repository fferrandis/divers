heap_pool: clean
	$(CC) -fpic  heap_pool.c -o libheap_pool.so -lrt -Wall -W -Wextra -shared  -O2 
	objcopy --only-keep-debug libheap_pool.so libheap_pool.so.debug

test: heap_pool
	$(CC) main.c -L. -lheap_pool -o main   -O2

clean:
	rm -f libheap_pool.so* main
