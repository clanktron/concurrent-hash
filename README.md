# Hash Hash Hash
> Clayton Castro, UID: 005412538 
This lab implements two different strategies of using multithreading in hash table operations.

## Building
```bash
make
```

## Running
The -t flag specifies the number of threads to use. Whereas the -s specifies the number of entries in the hash map.
```bash
./hash-table-tester -t 5 -s 50000
# Generation: 34,723 usec
# Hash table base: 415,525 usec
#   - 0 missing
```

## First Implementation
In the `hash_table_v1_add_entry` function, a single mutex is used to ensure safe concurrency. The locking is done at the beginning of the function and unlocked at the end of such. This ensures that any hash table manipulations are done by only one thread at a time.

### Performance
```bash
./hash-table-tester -t 5 -s 50000
# Generation: 34,723 usec
# Hash table base: 415,525 usec
#   - 0 missing
# Hash table v1: 884,506 usec
#   - 0 missing
```
The execution time taken for implementation 1 is slower than the base case as the overhead of context switching slows down the hash table operations.

## Second Implementation
In the `hash_table_v2_add_entry` function, multiple mutexes are used to allow for more efficient multithreading. Locks are placed around operations that deal with the head of each linked list in order to preserve correctness, however other parts (like the manipulation of attributes of members of said linked list) are left outside the section as they are unaffected by concurrency.

### Performance
```bash
./hash-table-tester -t 5 -s 50000
# Generation: 34,723 usec
# Hash table base: 415,525 usec
#   - 0 missing
# Hash table v1: 884,506 usec
#   - 0 missing
# Hash table v2: 104,268 usec
#   - 0 missing
```
This second implementation is faster as it allows for more non-critical operations to be executed concurrently, as opposed to locking the entire function as in the first implementation.

## Cleaning up
```bash
make clean
```
