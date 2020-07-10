# Flakes

## FollowSuite

```
#172 79.10 === RUN   TestFollowSuite/TestStreaming
#172 79.10     TestFollowSuite/TestStreaming: follow_test.go:126:
#172 79.10         	Error Trace:	follow_test.go:126
#172 79.10         	Error:      	Not equal:
#172 79.10         	            	expected: []byte{0x61, 0x62, 0x63, 0x64, 0x65, 0x66, 0x67, 0x68, 0x69, 0x6a, 0x6b, 0x6c, 0x6d, 0x6e, 0x6f}
#172 79.10         	            	actual  : []byte{0x61, 0x62, 0x63, 0x64, 0x65, 0x66, 0x67, 0x68, 0x69}
#172 79.10
#172 79.10         	            	Diff:
#172 79.10         	            	--- Expected
#172 79.10         	            	+++ Actual
#172 79.10         	            	@@ -1,3 +1,3 @@
#172 79.10         	            	-([]uint8) (len=15) {
#172 79.10         	            	- 00000000  61 62 63 64 65 66 67 68  69 6a 6b 6c 6d 6e 6f     |abcdefghijklmno|
#172 79.10         	            	+([]uint8) (len=9) {
#172 79.10         	            	+ 00000000  61 62 63 64 65 66 67 68  69                       |abcdefghi|
#172 79.10         	            	 }
#172 79.10         	Test:       	TestFollowSuite/TestStreaming
#172 79.10 === RUN   TestFollowSuite/TestStreamingClose
#172 79.10 === RUN   TestFollowSuite/TestStreamingSmallBuffer
#172 79.10 === RUN   TestFollowSuite/TestStreamingWithSomeHead
#172 79.10 --- FAIL: TestFollowSuite (0.70s)
#172 79.10     --- PASS: TestFollowSuite/TestDeleted (0.11s)
#172 79.10     --- PASS: TestFollowSuite/TestReadWrite (0.05s)
#172 79.10     --- FAIL: TestFollowSuite/TestStreaming (0.11s)
#172 79.10     --- PASS: TestFollowSuite/TestStreamingClose (0.15s)
#172 79.10     --- PASS: TestFollowSuite/TestStreamingSmallBuffer (0.10s)
#172 79.10     --- PASS: TestFollowSuite/TestStreamingWithSomeHead (0.15s)
#172 79.10 FAIL
```
