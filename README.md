# R3

R3 is a general-purpose record and replay tool which is developed primarily
for the FIRST Robots Competition (FRC). It stands for Robotic Record and Replay,
but is not necessarily made just for Robotics.

It works by logging function calls in a binary file, and then reading out of it
at the correct times. This simple solution can ease several parts in the process
of creating autonomous code for a robot.

> This library is not production ready in any shape or form. **Do not use it unless
> You like taking risks!**

## Usage

The API was designed to be as easy and general as possible, so these explanations
will be short.

### Marking functions to be recorded

This is probably the hardest part in this library, and that's saying something. Note
that this part is likely to change in the future to make it easier; what follows is the
current API.

To do this, you need to call the `Record.recordCall` function with a `Signature` object
and the parameters passed into the function. A `Signature` object is pretty simple to
create: pass the name of the function and the types of its arguments. An example will
illustrate this:

```java
class MyLib {
	String myFunc(String a, Integer b) {
		Record.recordCall(new Signature("MyLib.myFunc", a.class, b.class), a, b)
		// Implementation...
	}
}
```

Of course, `a.class` and `b.class` could easily be replaced with their respective class
names.

**Important Limitation:** any class which has such a function being recorded *must* define
a static `getInstance` method which returns an instance of that class or null otherwise.
This is used to call the function during the replay, and although I hope that this will
be fixed in the future, it is staying for now.

### Recording

To start recording, call `Record.start`. To stop recording, call `Record.stop`. Recordings
will be written to `recording.bin` by default, but this can be changed by assigning to
`Record.recordingFile`. Here's an example:

```java
Record.recordingFile = "test-recording.bin";
Record.start();
func_call();
Record.stop();
```

### Replaying

Replaying is just as simple as recording: call `Replay.replay` on a filename to run the
recording stored in that file. Easy as that. Example:

```java
Replay.replay("test-recording.bin");
```