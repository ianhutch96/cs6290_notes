# Many Cores

Two major challenges encountered in implementing a multi­core system are:

1. ­As the number of cores increases, coherence traffic increases. This requires:
    - A scalable on-­chip network
    - Directory Coherence
2. As the number of cores increases, the bus traffic increases to the point of bottleneck. To address this:
    - Use a mesh connection rather than a bus.
3. Off chip traffic increases while the number of pins on the chip increases but not at the same rate. To solve this:
    - **The last level cache is shared and distributed among all the cores and its size increases with each core.**
4. The power budget available for a chip must be split between cores. Thus:
    - The frequency and voltage must be reduced.
5. Operating system confusion caused by multi­threading and cache sharing between cores.
    - The OS needs to know on which core to run the thread to obtain the best performance.

## On-Chip Network

- **Instead of a bus, use a mesh connection**. This will increase the throughput of the entire network.
- With a mesh, each additional core increases the number of connections to the network.
- Torus Networks are a wrapped mesh network.

## On-Chip Directory

- The Coherence Directory becomes too large when there are many cores. So the on­chip directory is sliced and distributed to each core. **The directory is sliced the same as the LLC**.
- **Partial Directory**: a limited number of entries reserved for blocks that are in at least one cache.
- When a directory becomes full, use an LRU protocol to replace an entry. This type of miss is caused by invalidation due to a replacement.

## Distributed LLC

The distributed last level cache (LLC) is sliced up and controlled by each core via one of the two methods:

1. ­Round robin: Not good for locality
2. ­Round robin with page numbers: Better for locality

## Multi­Core Power and Performance

The more cores, the slower each core can operate. To combat this, the cores that are operating get a boost of power and frequency.
