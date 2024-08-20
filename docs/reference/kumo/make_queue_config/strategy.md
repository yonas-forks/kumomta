# strategy

{{since('dev')}}

Optional string to specify the scheduled queue strategy.  There are two possible
values:

* `"TimerWheel"` - the default. The timer wheel has `O(1)` insertion and `O(1)`
  pop costs, making it good for very large scheduled queues, but that comes in
  exchange for a flat periodic tick overhead.  As the number of scheduled queues
  increases and/or the `retry_interval` decreases, so does the aggregate overhead
  of maintaining timerwheel queues.
* `"SkipList"` - A skiplist has `O(log n)` insertion and `O(1)` pop costs,
  making it a good general purpose queue, but becomes more expensive to insert
  into as the size of the queue increases.  That higher insertion cost is in
  exchange for the overall maintenance being cheaper, as the queue can go to
  sleep until the time of the next due item.  The ongoing and aggregate
  maintenance is therefore cheaper than a `TimerWheel` but the worst-case
  scenario where the destination is not accepting mail and causing the
  scheduled queue to grow is logarithmically more expensive as the queue
  grows.

Which should you use? Whichever works best for your environment! Make sure that
you test normal healthy operation with a lot of queues as well as the worst
case scenario where those queues are full and egress is blocked.

If you have very short `retry_interval` set for the majority of your queues you
may wish to adopt `SkipList` for its lower idle CPU cost, or alternatively use
`TimerWheel` and find a `timerwheel_tick_interval` that works for your typical
number of queues.

!!! note
    Changing the strategy for a given queue requires that the queue either be
    aged out, or for kumod to be restarted, before it will take effect.  This
    restriction may be removed in a future release.

