---
title: Memsql is lock free, but how?
---

Had memsql come by and give architecture presentation yesterday so i started reading [http://blog.memsql.com/common-pitfalls-in-writing-lock-free-algorithms/](http://blog.memsql.com/common-pitfalls-in-writing-lock-free-algorithms/) … complicated, and no detailed explanation of how it works anywhere! Kinda need to know your c/asm… and in the end, you get to things like “lock; cmpxchg16b” (mentioned in blog and here [https://github.com/memsql/lockfree-bench/blob/0a82132d21d3779820720d5591f2b3bae9f6b65f/stack/boost/atomic/detail/gcc-x86.hpp#L1443](https://github.com/memsql/lockfree-bench/blob/0a82132d21d3779820720d5591f2b3bae9f6b65f/stack/boost/atomic/detail/gcc-x86.hpp#L1443) )

It’s a hardware assisted atomic operation (two of them – compare, and swap). Neat. There’s still cache sync overhead in hardware (actually pretty huge relatively speaking), but what can we do… I just wish this was explained better somewhere.

Also, as this article summarizes, [http://moodycamel.com/blog/2013/a-fast-lock-free-queue-for-c++](http://moodycamel.com/blog/2013/a-fast-lock-free-queue-for-c++)
“the cache coherence protocols and memory barrier instructions don’t seem to scale very well” – so shared-memory multi-CPU architectures will hit the wall too, and we’ll need new type of memory architecture to support in-memory databases. It will still lock, but better 🙂
In fact, the latest and greatest Intel CPU already got some instructions for this, [http://en.wikipedia.org/wiki/Transactional_Synchronization_Extensions](http://en.wikipedia.org/wiki/Transactional_Synchronization_Extensions)