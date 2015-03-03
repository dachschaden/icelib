# icelib
Function collection to make a programmer's life a tiny bit easier.

Q: What is icelib?

A: It's a collection of interfaces that I once hacked to observe and download
4chan.org thread images. Actually I wrote an awful lot of C code and then
wanted to use it in other projects as well, and then realised that I did a
shitty job in abstracting my code. So I tried to split everything up and
created data structures and hacked functions that would automatically clear
memory (which is a "feature" in my opinon, reserving dynamic memory in your
subpointers and get it freed optionally when you clear your whole object). And
since I decided to become better in C as well I decided to put in some
additional code for some network tasks, such as IRC and SOCKS support,
persistent memory, DNS and so on.

For beginners the API might be hard to use, and I just didn't care enough for
any backwards compatibility for ancient operating systems, so be aware: it
compiles on Linux, I dunno about other kernels. It compiles on x86 as well as
on x86-64, and it runs "stable" on both plattforms. The main goal of the API
was to give you an interface that would do exactly what you imagine it to do,
in the most easy way while trying to avoid as much bloat as possible (so much
that it also provides an API to count ticks - I used it frequently and am
still using it to improve the code).

==============================================================================

Q: What can I do with it?

A: Writing web crawlers and IRC bots, so far. Actually you can do even more,
but these are the two main goals that I wanted to achieve. It as well supports
multi-threading (not to confuse with multi-processing - LWPs are used instread
of forks). You could as well write servers with it (although some code would
need some alternations, I guess). It is not really recommended though, since I
am still using select instead of epoll - for high performance server select
does not scale very well. I am aware of that, but I yet have to think of the
best way to implement a server with icelib.

I tried my best to avoid bloat and bottlenecks. For example some functions in
icelib support persistent memory objects. Instead that, for example,
the icelib_send_request uses its own memory on the heap to store the response,
frees it afterwards, and the icelib_gzip_decode uses its own memory AGAIN, you
can pass both the same icelib_heap object. They will then use the same memory,
they won't have to malloc and free their temp buffers, and the CPU load
decreases dramatically (on I/O bound test programs from 7% load to 0% load).
Icelib tries to consume memory more than other resources - it's primarily
I/O bound, secondarily memory bound.
