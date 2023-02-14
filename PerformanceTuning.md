https://sematext.com/blog/jvm-performance-tuning/

Memory footprint - The memory occupied, or required for triggering GC

Latency - The time taken to trigger the GC

Though put - The ratio of time spent in application vs GC


GC logs - monitor the memory, Latency, through put

Garbage Collector - Serial,Parallel,MarkSweep,G1

G1GC - does parallel GC of young and old gen and defer certain operations to reduce the stop_the_world event

old gen - long living objects

young gen - short living objects
