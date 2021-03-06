version: 1.9.2
## package log

  `import "log"`

## Overview

Package log implements a simple logging package. It defines a type, Logger, with
methods for formatting output. It also has a predefined 'standard' Logger
accessible through helper functions Print[f|ln], Fatal[f|ln], and Panic[f|ln],
which are easier to use than creating a Logger manually. That logger writes to
standard error and prints the date and time of each logged message. Every log
message is output on a separate line: if the message being printed does not end
in a newline, the logger will add one. The Fatal functions call os.Exit(1) after
writing the log message. The Panic functions call panic after writing the log
message.

## Index

- [Constants](#pkg-constants)
- [func Fatal(v ...interface{})](#Fatal)
- [func Fatalf(format string, v ...interface{})](#Fatalf)
- [func Fatalln(v ...interface{})](#Fatalln)
- [func Flags() int](#Flags)
- [func Output(calldepth int, s string) error](#Output)
- [func Panic(v ...interface{})](#Panic)
- [func Panicf(format string, v ...interface{})](#Panicf)
- [func Panicln(v ...interface{})](#Panicln)
- [func Prefix() string](#Prefix)
- [func Print(v ...interface{})](#Print)
- [func Printf(format string, v ...interface{})](#Printf)
- [func Println(v ...interface{})](#Println)
- [func SetFlags(flag int)](#SetFlags)
- [func SetOutput(w io.Writer)](#SetOutput)
- [func SetPrefix(prefix string)](#SetPrefix)
- [type Logger](#Logger)
  - [func New(out io.Writer, prefix string, flag int) *Logger](#New)
  - [func (l *Logger) Fatal(v ...interface{})](#Logger.Fatal)
  - [func (l *Logger) Fatalf(format string, v ...interface{})](#Logger.Fatalf)
  - [func (l *Logger) Fatalln(v ...interface{})](#Logger.Fatalln)
  - [func (l *Logger) Flags() int](#Logger.Flags)
  - [func (l *Logger) Output(calldepth int, s string) error](#Logger.Output)
  - [func (l *Logger) Panic(v ...interface{})](#Logger.Panic)
  - [func (l *Logger) Panicf(format string, v ...interface{})](#Logger.Panicf)
  - [func (l *Logger) Panicln(v ...interface{})](#Logger.Panicln)
  - [func (l *Logger) Prefix() string](#Logger.Prefix)
  - [func (l *Logger) Print(v ...interface{})](#Logger.Print)
  - [func (l *Logger) Printf(format string, v ...interface{})](#Logger.Printf)
  - [func (l *Logger) Println(v ...interface{})](#Logger.Println)
  - [func (l *Logger) SetFlags(flag int)](#Logger.SetFlags)
  - [func (l *Logger) SetOutput(w io.Writer)](#Logger.SetOutput)
  - [func (l *Logger) SetPrefix(prefix string)](#Logger.SetPrefix)

### Examples

- [Logger](#exampleLogger)
- [Logger.Output](#exampleLogger_Output)

### Package files
 [log.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// Bits or&#39;ed together to control what&#39;s printed.</span>
    <span class="comment">// There is no control over the order they appear (the order listed</span>
    <span class="comment">// here) or the format they present (as described in the comments).</span>
    <span class="comment">// The prefix is followed by a colon only when Llongfile or Lshortfile</span>
    <span class="comment">// is specified.</span>
    <span class="comment">// For example, flags Ldate | Ltime (or LstdFlags) produce,</span>
    <span class="comment">//	2009/01/23 01:23:23 message</span>
    <span class="comment">// while flags Ldate | Ltime | Lmicroseconds | Llongfile produce,</span>
    <span class="comment">//	2009/01/23 01:23:23.123123 /a/b/c/d.go:23: message</span>
    <span id="Ldate">Ldate</span>         = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>     <span class="comment">// the date in the local time zone: 2009/01/23</span>
    <span id="Ltime">Ltime</span>                         <span class="comment">// the time in the local time zone: 01:23:23</span>
    <span id="Lmicroseconds">Lmicroseconds</span>                 <span class="comment">// microsecond resolution: 01:23:23.123123.  assumes Ltime.</span>
    <span id="Llongfile">Llongfile</span>                     <span class="comment">// full file name and line number: /a/b/c/d.go:23</span>
    <span id="Lshortfile">Lshortfile</span>                    <span class="comment">// final file name element and line number: d.go:23. overrides Llongfile</span>
    <span id="LUTC">LUTC</span>                          <span class="comment">// if Ldate or Ltime is set, use UTC rather than the local time zone</span>
    <span id="LstdFlags">LstdFlags</span>     = <a href="#Ldate">Ldate</a> | <a href="#Ltime">Ltime</a> <span class="comment">// initial values for the standard logger</span>
)</pre>

These flags define which text to prefix to each log entry generated by the
Logger.

<h2 id="Fatal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L295">Fatal</a>
    <a href="#Fatal">¶</a></h2>
<pre>func Fatal(v ...interface{})</pre>

Fatal is equivalent to Print() followed by a call to os.Exit(1).

<h2 id="Fatalf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L301">Fatalf</a>
    <a href="#Fatalf">¶</a></h2>
<pre>func Fatalf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Fatalf is equivalent to Printf() followed by a call to os.Exit(1).

<h2 id="Fatalln">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L307">Fatalln</a>
    <a href="#Fatalln">¶</a></h2>
<pre>func Fatalln(v ...interface{})</pre>

Fatalln is equivalent to Println() followed by a call to os.Exit(1).

<h2 id="Flags">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L255">Flags</a>
    <a href="#Flags">¶</a></h2>
<pre>func Flags() <a href="/builtin/#int">int</a></pre>

Flags returns the output flags for the standard logger.

<h2 id="Output">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L340">Output</a>
    <a href="#Output">¶</a></h2>
<pre>func Output(calldepth <a href="/builtin/#int">int</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Output writes the output for a logging event. The string s contains the text to
print after the prefix specified by the flags of the Logger. A newline is
appended if the last character of s is not already a newline. Calldepth is the
count of the number of frames to skip when computing the file name and line
number if Llongfile or Lshortfile is set; a value of 1 will print the details
for the caller of Output.

<h2 id="Panic">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L313">Panic</a>
    <a href="#Panic">¶</a></h2>
<pre>func Panic(v ...interface{})</pre>

Panic is equivalent to Print() followed by a call to panic().

<h2 id="Panicf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L320">Panicf</a>
    <a href="#Panicf">¶</a></h2>
<pre>func Panicf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Panicf is equivalent to Printf() followed by a call to panic().

<h2 id="Panicln">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L327">Panicln</a>
    <a href="#Panicln">¶</a></h2>
<pre>func Panicln(v ...interface{})</pre>

Panicln is equivalent to Println() followed by a call to panic().

<h2 id="Prefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L265">Prefix</a>
    <a href="#Prefix">¶</a></h2>
<pre>func Prefix() <a href="/builtin/#string">string</a></pre>

Prefix returns the output prefix for the standard logger.

<h2 id="Print">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L278">Print</a>
    <a href="#Print">¶</a></h2>
<pre>func Print(v ...interface{})</pre>

Print calls Output to print to the standard logger. Arguments are handled in the
manner of fmt.Print.

<h2 id="Printf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L284">Printf</a>
    <a href="#Printf">¶</a></h2>
<pre>func Printf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Printf calls Output to print to the standard logger. Arguments are handled in
the manner of fmt.Printf.

<h2 id="Println">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L290">Println</a>
    <a href="#Println">¶</a></h2>
<pre>func Println(v ...interface{})</pre>

Println calls Output to print to the standard logger. Arguments are handled in
the manner of fmt.Println.

<h2 id="SetFlags">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L260">SetFlags</a>
    <a href="#SetFlags">¶</a></h2>
<pre>func SetFlags(flag <a href="/builtin/#int">int</a>)</pre>

SetFlags sets the output flags for the standard logger.

<h2 id="SetOutput">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L248">SetOutput</a>
    <a href="#SetOutput">¶</a></h2>
<pre>func SetOutput(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

SetOutput sets the output destination for the standard logger.

<h2 id="SetPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L270">SetPrefix</a>
    <a href="#SetPrefix">¶</a></h2>
<pre>func SetPrefix(prefix <a href="/builtin/#string">string</a>)</pre>

SetPrefix sets the output prefix for the standard logger.

<h2 id="Logger">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L40">Logger</a>
    <a href="#Logger">¶</a></h2>
<pre>type Logger struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Logger represents an active logging object that generates lines of output to
an io.Writer. Each logging operation makes a single call to the Writer's Write
method. A Logger can be used simultaneously from multiple goroutines; it
guarantees to serialize access to the Writer.

<a id="exampleLogger"></a>
Example:

    var (
        buf    bytes.Buffer
        logger = log.New(&buf, "logger: ", log.Lshortfile)
    )

    logger.Print("Hello, log file!")

    fmt.Print(&buf)
    // Output:
    // logger: example_test.go:19: Hello, log file!

<h3 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L52">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(out <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, prefix <a href="/builtin/#string">string</a>, flag <a href="/builtin/#int">int</a>) *<a href="#Logger">Logger</a></pre>

New creates a new Logger. The out variable sets the destination to which log
data will be written. The prefix appears at the beginning of each generated log
line. The flag argument defines the logging properties.

<h3 id="Logger.Fatal">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L181">Fatal</a>
    <a href="#Logger.Fatal">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Fatal(v ...interface{})</pre>

Fatal is equivalent to l.Print() followed by a call to os.Exit(1).

<h3 id="Logger.Fatalf">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L187">Fatalf</a>
    <a href="#Logger.Fatalf">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Fatalf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Fatalf is equivalent to l.Printf() followed by a call to os.Exit(1).

<h3 id="Logger.Fatalln">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L193">Fatalln</a>
    <a href="#Logger.Fatalln">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Fatalln(v ...interface{})</pre>

Fatalln is equivalent to l.Println() followed by a call to os.Exit(1).

<h3 id="Logger.Flags">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L220">Flags</a>
    <a href="#Logger.Flags">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Flags() <a href="/builtin/#int">int</a></pre>

Flags returns the output flags for the logger.

<h3 id="Logger.Output">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L139">Output</a>
    <a href="#Logger.Output">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Output(calldepth <a href="/builtin/#int">int</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Output writes the output for a logging event. The string s contains the text to
print after the prefix specified by the flags of the Logger. A newline is
appended if the last character of s is not already a newline. Calldepth is used
to recover the PC and is provided for generality, although at the moment on all
pre-defined paths it will be 2.

<a id="exampleLogger_Output"></a>
Example:

    var (
        buf    bytes.Buffer
        logger = log.New(&buf, "INFO: ", log.Lshortfile)

        infof = func(info string) {
            logger.Output(2, info)
        }
    )

    infof("Hello world")

    fmt.Print(&buf)
    // Output:
    // INFO: example_test.go:36: Hello world

<h3 id="Logger.Panic">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L199">Panic</a>
    <a href="#Logger.Panic">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Panic(v ...interface{})</pre>

Panic is equivalent to l.Print() followed by a call to panic().

<h3 id="Logger.Panicf">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L206">Panicf</a>
    <a href="#Logger.Panicf">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Panicf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Panicf is equivalent to l.Printf() followed by a call to panic().

<h3 id="Logger.Panicln">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L213">Panicln</a>
    <a href="#Logger.Panicln">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Panicln(v ...interface{})</pre>

Panicln is equivalent to l.Println() followed by a call to panic().

<h3 id="Logger.Prefix">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L234">Prefix</a>
    <a href="#Logger.Prefix">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Prefix() <a href="/builtin/#string">string</a></pre>

Prefix returns the output prefix for the logger.

<h3 id="Logger.Print">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L174">Print</a>
    <a href="#Logger.Print">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Print(v ...interface{})</pre>

Print calls l.Output to print to the logger. Arguments are handled in the manner
of fmt.Print.

<h3 id="Logger.Printf">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L168">Printf</a>
    <a href="#Logger.Printf">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Printf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Printf calls l.Output to print to the logger. Arguments are handled in the
manner of fmt.Printf.

<h3 id="Logger.Println">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L178">Println</a>
    <a href="#Logger.Println">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Println(v ...interface{})</pre>

Println calls l.Output to print to the logger. Arguments are handled in the
manner of fmt.Println.

<h3 id="Logger.SetFlags">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L227">SetFlags</a>
    <a href="#Logger.SetFlags">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) SetFlags(flag <a href="/builtin/#int">int</a>)</pre>

SetFlags sets the output flags for the logger.

<h3 id="Logger.SetOutput">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L57">SetOutput</a>
    <a href="#Logger.SetOutput">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) SetOutput(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

SetOutput sets the output destination for the logger.

<h3 id="Logger.SetPrefix">func (*Logger) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/log.go#L241">SetPrefix</a>
    <a href="#Logger.SetPrefix">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) SetPrefix(prefix <a href="/builtin/#string">string</a>)</pre>

SetPrefix sets the output prefix for the logger.

## Subdirectories
- [..](..)
- [syslog](syslog/)
