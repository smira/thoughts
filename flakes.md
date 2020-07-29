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

## Containerd.RunTwice

```
#172 21.89 === RUN   TestContainerdSuite/TestRunTwice
#172 21.89 2020/07/10 18:57:24 state Running: Started task 036de7a9-f667-48e8-905f-216233980f94 (PID 4964) for container 036de7a9-f667-48e8-905f-216233980f94
#172 21.89     TestContainerdSuite/TestRunTwice: containerd_test.go:179:
#172 21.89         	Error Trace:	containerd_test.go:179
#172 21.89         	Error:      	Received unexpected error:
#172 21.89         	            	failed to create task: "036de7a9-f667-48e8-905f-216233980f94": dial unix \00/containerd-shim/f88fff9fe9795db4229846b09b2da816f6bd981b8112345486ff3b5653892920.sock: connect: connection refused: unknown
#172 21.89         	Test:       	TestContainerdSuite/TestRunTwice
#172 21.89 --- FAIL: TestContainerdSuite (6.62s)
#172 21.89     --- PASS: TestContainerdSuite/TestContainerCleanup (0.92s)
#172 21.89     --- PASS: TestContainerdSuite/TestImportFail (0.54s)
#172 21.89     --- PASS: TestContainerdSuite/TestImportSuccess (0.45s)
#172 21.89     --- PASS: TestContainerdSuite/TestRunLogs (0.26s)
#172 21.89     --- PASS: TestContainerdSuite/TestRunSuccess (0.31s)
#172 21.89     --- FAIL: TestContainerdSuite/TestRunTwice (0.39s)
#172 21.89     --- PASS: TestContainerdSuite/TestStopFailingAndRestarting (1.61s)
#172 21.89     --- PASS: TestContainerdSuite/TestStopSigKill (0.95s)
#172 21.89 FAIL
```

## TestFollowSuite/TestDeleted

```
#167 97.51 === RUN   TestFollowSuite
#167 97.51 === RUN   TestFollowSuite/TestDeleted
#167 97.51     follow_test.go:228:
#167 97.51         	Error Trace:	follow_test.go:228
#167 97.51         	Error:      	Not equal:
#167 97.51         	            	expected: []byte{0x61, 0x62, 0x63, 0x64, 0x65, 0x66, 0x67, 0x68, 0x69, 0x6a, 0x6b, 0x6c, 0x6d, 0x6e, 0x6f}
#167 97.51         	            	actual  : []byte{0x61, 0x62, 0x63}
#167 97.51
#167 97.51         	            	Diff:
#167 97.51         	            	--- Expected
#167 97.51         	            	+++ Actual
#167 97.51         	            	@@ -1,3 +1,3 @@
#167 97.51         	            	-([]uint8) (len=15) {
#167 97.51         	            	- 00000000  61 62 63 64 65 66 67 68  69 6a 6b 6c 6d 6e 6f     |abcdefghijklmno|
#167 97.51         	            	+([]uint8) (len=3) {
#167 97.51         	            	+ 00000000  61 62 63                                          |abc|
#167 97.51         	            	 }
#167 97.51         	Test:       	TestFollowSuite/TestDeleted
```
