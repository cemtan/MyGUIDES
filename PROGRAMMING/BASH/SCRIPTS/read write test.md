#### READ WRITE TEST
---
---


```bash
#!/bin/bash

#DD
#WRITE
while (true); do
	number=$((($RANDOM * 32768 + $RANDOM)))
	#dd if=/dev/urandom of=/u02/test seek=$number count=1
	dd if=/dev/urandom of=/u02/test bs=1M count=100 conv=fdatasync
	#we pass the conv=fdatasync options to the command to ensure the disk is written to the disk physically, and not in the buffer at completion.
done

#READ
while (true); do
	number=$((($RANDOM * 32768 + $RANDOM)))
	#dd if=/dev/urandom of=/u02/test seek=$number count=1
	dd if=/u02/test of=/dev/null bs=1M iflag=direct
done
```