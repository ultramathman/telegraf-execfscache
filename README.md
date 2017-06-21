# telegraf-execfscache
Simple fscache collector script for the Telegraf exec input
## Configuration
Sample telegraf configuration file, place in /etc/telegraf/telegraf.d/fscache.conf

```
[[inputs.exec]]
  # Shell/commands array
  # compatible with old version
  # we can still use the old command configuration
  # command = "/usr/bin/line_protocol_collector"
  commands = ["/usr/local/bin/telegraf-fscache"]
  ## Timeout for each command to complete.
  timeout = "5s"
  # Data format to consume.
  # NOTE json only reads numerical measurements, strings and booleans are ignored.
  data_format = "influx"

```
## Measurements & Fields:
The following fields are collected as floats under the execfscache measurement. Descriptions per [kernel documentation](https://www.kernel.org/doc/Documentation/filesystems/caching/fscache.txt):
Items with descriptions in the following are stored as fields, headings as tags:
Cookies:
idx -  Number of index cookies allocated
dat -  Number of data storage cookies allocated
spc -  Number of special cookies allocated
Objects:
alc -  Number of objects allocated
nal -  Number of object allocation failures
avl -  Number of objects that reached the available state
ded -  Number of objects that reached the dead state
ChkAux:
non -  Number of objects that didn't have a coherency check
ok -  Number of objects that passed a coherency check
upd -  Number of objects that needed a coherency data update
obs -  Number of objects that were declared obsolete
Pages:
mrk -  Number of pages marked as being cached
unc -  Number of uncache page requests seen
Acquire:
n -  Number of acquire cookie requests seen
nul -  Number of acq reqs given a NULL parent
noc -  Number of acq reqs rejected due to no cache available
ok -  Number of acq reqs succeeded
nbf -  Number of acq reqs rejected due to error
oom -  Number of acq reqs failed on ENOMEM
Lookups:
n -  Number of lookup calls made on cache backends
neg -  Number of negative lookups made
pos -  Number of positive lookups made
crt -  Number of objects created by lookup
tmo -  Number of lookups timed out and requeued
Updates:
n -  Number of update cookie requests seen
nul -  Number of upd reqs given a NULL parent
run -  Number of upd reqs granted CPU time
Relinqs:
n -  Number of relinquish cookie requests seen
nul -  Number of rlq reqs given a NULL parent
wcr -  Number of rlq reqs waited on completion of creation
AttrChg:
n -  Number of attribute changed requests seen
ok -  Number of attr changed requests queued
nbf -  Number of attr changed rejected -ENOBUFS
oom -  Number of attr changed failed -ENOMEM
run -  Number of attr changed ops given CPU time
Allocs:
n -  Number of allocation requests seen
ok -  Number of successful alloc reqs
wt -  Number of alloc reqs that waited on lookup completion
nbf -  Number of alloc reqs rejected -ENOBUFS
int -  Number of alloc reqs aborted -ERESTARTSYS
ops -  Number of alloc reqs submitted
owt -  Number of alloc reqs waited for CPU time
abt -  Number of alloc reqs aborted due to object death
Retrvls:
n -  Number of retrieval (read) requests seen
ok -  Number of successful retr reqs
wt -  Number of retr reqs that waited on lookup completion
nod -  Number of retr reqs returned -ENODATA
nbf -  Number of retr reqs rejected -ENOBUFS
int -  Number of retr reqs aborted -ERESTARTSYS
oom -  Number of retr reqs failed -ENOMEM
ops -  Number of retr reqs submitted
owt -  Number of retr reqs waited for CPU time
abt -  Number of retr reqs aborted due to object death
Stores:
n -  Number of storage (write) requests seen
ok -  Number of successful store reqs
agn -  Number of store reqs on a page already pending storage
nbf -  Number of store reqs rejected -ENOBUFS
oom -  Number of store reqs failed -ENOMEM
ops -  Number of store reqs submitted
run -  Number of store reqs granted CPU time
pgs -  Number of pages given store req processing time
rxd -  Number of store reqs deleted from tracking tree
olm -  Number of store reqs over store limit
VmScan:
nos -  Number of release reqs against pages with no pending store
gon -  Number of release reqs against pages stored by time lock granted
bsy -  Number of release reqs ignored due to in-progress store
can -  Number of page stores cancelled due to release req
Ops:
pend -  Number of times async ops added to pending queues
run -  Number of times async ops given CPU time
enq -  Number of times async ops queued for processing
can -  Number of async ops cancelled
rej -  Number of async ops rejected due to object lookup/create failure
ini -  Number of async ops initialised
dfr -  Number of async ops queued for deferred release
rel -  Number of async ops released (should equal ini=N when idle)
gc -  Number of deferred-release async ops garbage collected
CacheOp:
alo -  Number of in-progress alloc_object() cache ops
luo -  Number of in-progress lookup_object() cache ops
luc -  Number of in-progress lookup_complete() cache ops
gro -  Number of in-progress grab_object() cache ops
upo -  Number of in-progress update_object() cache ops
dro -  Number of in-progress drop_object() cache ops
pto -  Number of in-progress put_object() cache ops
syn -  Number of in-progress sync_cache() cache ops
atc -  Number of in-progress attr_changed() cache ops
rap -  Number of in-progress read_or_alloc_page() cache ops
ras -  Number of in-progress read_or_alloc_pages() cache ops
alp -  Number of in-progress allocate_page() cache ops
als -  Number of in-progress allocate_pages() cache ops
wrp -  Number of in-progress write_page() cache ops
ucp -  Number of in-progress uncache_page() cache ops
dsp -  Number of in-progress dissociate_pages() cache ops
CacheEv:
nsp -  Number of object lookups/creations rejected due to lack of space
stl -  Number of stale objects deleted
rtr -  Number of objects retired when relinquished
cul -  Number of objects culled

## Tags:
The measurements have the following tags:
  * host - the host where data was collected
  * class - one of {cookies,objects,chkaux,pages,acquire,lookups,updates,relinqs,attrchg,allocs,retrvls,stores,vmscan,ops,cacheop,cacheev}
