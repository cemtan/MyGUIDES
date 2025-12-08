#### IOZONE
---
---



To run random read-and-write tests, we can pass the -i2 option flag to the iozone command:

	iozone -t1 -i0 -i2 -r1k -s1g /tmp

In addition to the –i2 option flag, we’ve also configured the tests using different option flags. Firstly, the -t1 option set the number of threads for test execution to one. Then, we specify the -i0 to make iozone create the test file for a test. Furthermore, the -r1k and -s1g configure the test to use a block size of 1 kilobyte with a total size of 1 gigabyte. Finally, we set the test path to /tmp.
'
