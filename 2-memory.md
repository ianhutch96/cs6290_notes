# Memory

## Memory Technology

- There are two memory technologies, Static Random Access Memory (SRAM) and Dynamic Random Access Memory (DRAM)
- Random Access means we can arbitrarily access any part of the memory
- Static means it retains its data as long as power is supplied
- Dynamic means it will lose the data if it isn't refreshed
- DRAM only needs one transistor per bit (less expensive)
- SRAM is more expensive and faster while DRAM is cheaper and slower

### Memory Chip Organization

- Memory is organized and accessed using Row Decoders and Column Decoders.
- A row can be written and read at the same time, this is called fast page mode. Using the fast page mode, memory can be read in a more efficient order.
- DRAM has destructive reads, meaning really we need to read-then-write. Writes in DRAM are also a read and then write operation.

### Fast Page Mode

- In traditional DRAM, when the CPU requests data, the memory controller has to wait for the row and column addresses to be supplied before data can be read or written.
- Fast Page Mode optimizes this process by allowing the memory controller to keep the row address constant while varying the column address to access different memory locations within the same row. This reduces the time needed to access consecutive memory locations within the same row, as the row doesn’t need to be refreshed for each access.
- Can do more than one operation between opening and closing a page.

### Connecting the DRAM to the Processor

- The processor send a request to L1, L1 sends its misses and write back requests to the larger L2, L2 does the same for an even larger L3 cache, and so on. These are all on the processor chip. The misses and the write back requests from the LLC are made over an external connection (processor pins, traditionally called a Front-Side-Bus) to the memory controller. The memory controller will have a memory channel which connects it to a DRAM module. Over this channel it issues requests like open a row, read something and get the data, and so on. The memory controller usually has multiple memory channel connections to DRAM modules. The memory latency seen by the LLC cache is not only the access time of the DRAM module, it includes sending the request over the front side bus, having the memory controller decode it, sending a page to the appropriate DRAM module, getting the data back over the memory channel, and so on. This can be a significant part of the overall memory latency.
- The chip should be designed so it can connect to many possible DRAM modules.
- Recent processor chips integrate the memory controller, meaning the memory controller is put on the same chip as the processor and the caches. This makes it so we do not need the front-side-bus, which reduces the latency. However, the cost of this is that the processor chip is designed to talk to a specific DRAM module. This makes it so we need a high degree of standardization of DRAM module design. E.g. when we go from 4 GB to 8 GB DRAM we don't want to totally redesign everything.
