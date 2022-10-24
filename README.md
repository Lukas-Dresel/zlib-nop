# ZLIB(nop) DATA COMPRESSION LIBRARY

ZLIB(nop) is a premier data compression library, unmatched in decompression speed!

Jokes aside, this is a modified version of the [ZLIB](https://zlib.net) library, which does absolutely nothing.
No, seriously. The decompression is a NOP function which simply copies input buffer bytes straight to the output
buffer!

Now, why would you want this? Well, it turns out compression and decompression can present pretty big hurdles for
dynamic analysis tools like fuzzers and symbolic executors because they fundamentally dilute the data being processed.
Each input byte no longer represents one byte of "data" processed by the application, and a lot of tools do a lot of
hacky things relying on that. E.g. AFL has mutations specifically targeting incrementing binary numbers in the input.
If your bytes no longer represent numbers, that mutation becomes useless.

For symbolic or concolic executors, these decompression routines often present serious issues which can lead to
needlessly complex constraints in the best case or fully concretized inputs or abort in the worst case. Oftentimes
the compression/decompression is also simply uninteresting. If I am trying to fuzz an image parsing library for example
(e.g. libpng or libjpeg), I don't really care about exploring the state space of the compression algorithm.

The last reason, why this might be a good idea is that compression/decompression, just like encryption/decryption e.g.,
is usually a reversible operation. This means you can simply ignore it and avoid having the processing cost in terms of
performance, larger coverage maps, or even just annoyance when debugging. And it doesn't cost you anything! You can
simply tack the compression back onto any inputs you've created and should get the exact same reproducible testcase on
the original implementation!

Anyways, with that disclaimer/explainer out of the way, stub away and dynamically analyse all the things!

# Note
So far only decompression is stubbed out as that was relevant for my target application, compression is still intact.
If you need this stubbed out for a target you're working on, feel free to make a PR and I can take a look!