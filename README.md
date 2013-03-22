# The (Towers of) HanoiDB Backend for Riak/KV

This storage engine can function as an alternative backend for Basho's Riak/KV.
See also: [HanoiDB](https://github.com/basho/hanoidb)

### Configuration options
Put these values in your `app.config` in the `hanoidb` section

```erlang
 {hanoidb, [
          {data_root, "./data/hanoidb"},

          %% Enable/disable on-disk compression.
          %% {compress, none | gzip | snappy | lz4},
          {compress, none},

          %% Expire (automatically delete) entries after N seconds.
          %% When this value is 0 (zero), entries never expire.
          {expiry_secs, 0},

          %% Sync strategy `none' only syncs every time the
          %% nursery runs full, which is currently hard coded
          %% to be every 256 inserts or deletes.
          %%
          %% Sync strategy `sync' will sync the nursery log
          %% for every insert or delete operation.
          %% {sync_strategy, none | sync | {seconds, N}},
          {sync_strategy, none},

          %% The page size is a minimum page size, when a page fills
          %% up to beyond this size, it is written to disk.
          %% Compression applies to such units of page size.
          {page_size, 8192},

          %% Read/write buffer sizes apply to merge processes.
          %% A merge process has two read buffers and a write
          %% buffer, and there is a merge process *per level* in
          %% the database.
          {write_buffer_size, 524288},  % 512kB
          {read_buffer_size, 524288},  % 512kB

          %% The merge strategy is one of `fast' or `predictable'.
          %% Both have same log2(N) worst case, but `fast' is
          %% sometimes faster; yielding latency fluctuations.
          %% {merge_strategy, fast | predictable}
          {merge_strategy, fast}
         ]},
```

### How to deploy HanoiDB as a Riak/KV backend

Deploy `hanoidb` into a Riak devrel cluster using the `enable-hanoidb`
script. Clone the `riak` repo, change your working directory to it, and then
execute the `enable-hanoidb` script. It adds `hanoidb` as a dependency, runs
`make all devrel`, and then modifies the configuration settings of the
resulting dev nodes to use the hanoidb storage backend.

1. `git clone git://github.com/basho/riak.git`
1. `mkdir riak/deps`
1. `cd riak/deps`
1. `git clone git://github.com/basho-labs/riak_kv_hanoidb_backend.git`
1. `cd ..`
1. `./deps/riak_kv_hanoidb_backend/enable-hanoidb`
