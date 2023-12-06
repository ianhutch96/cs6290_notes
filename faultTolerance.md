# Fault Tolerance

## Dependability

- **Dependability** = quality of a delivered service that justifies relying on the system to provide the service.
- **Specified Service** = the expected system behavior
- **Delivered Service** = the actual system behavior
- System Modules have an ideal expected behavior. When the behavior deviates from the ideal the system to no longer provides the expected service.

## Faults, Errors, Failures

- **Fault** = module deviates from specified behavior (e.g. programming mistake)
- **Error** = actual behavior differs from expected behavior
- **Failure** = system deviates from specified behavior

- **Latent Error** = an error that occurs only when a specific task is performed.
- **Effective Error** = a latent error that has occurred, an activated fault.
- **Failure** = when the system deviates from the expected behavior.

## Faults, Errors, and Failures Example

- Consider an ADD function works in all cases, except the case 5 + 3 = 7.
- If the ADD function is never asked to produce the answer to 5 + 3, this is a fault
that is never activated, so there is no error. Thus, **a fault is needed to produce an error, but not every fault becomes an error**.
- If the ADD function produces the answer 5 + 3 = 7, but the answer is never used. This is an error that is not a failure. Thus, **there can be an error in a system, and never have a failure**.

## Reliability and Availability

- We can measure reliability. To measure reliability consider the system to be in one of two states.
  1. **Service accomplishment** - normal state, providing the service expected
  2. **Service interruption** - the service not being provided
- **Reliability is the measure of the continuous service accomplishment**
- A typical measurement of reliability is **Mean Time To Failure (MTTF)** which is how long will the system provide service before the next service interruption.
- **Availability** measures service accomplishment as a fraction of overall time.
- For example
  - If a system provides service for one year and has service interruption for one year.
    - The availability for the system is: 50%
    - The reliability for the system is 1 year.
  - If the system provides one month of service, then one month of service interruption for two years.
    - The availability is 50%
    - the reliability is 1 month
- **Mean Time to Repair (MTTR)** is the time it takes until a service is restored when a service interruption occurs.
$$ Availability = \frac{MTTF}{MTTF+MTTR}$$


## Kinds of Faults

1. **Faults Classified by Cause**:
   - Hardware Fault (hardware components fail to perform as designed)
   - Design Faults (software bugs, hardware design)
   - Operation Fault (operator and user mistakes)
   - Environmental Fault (fire, power failure, etc)
2. **Fault Classified by Duration**
   - Permanent Fault (cannot not be corrected)
   - Intermittent Fault (recurring fault)
   - Transient Fault (fault occurs and does not occur again)

## Improving Reliability and Availability

1. **Fault Avoidance**
   - Prevent faults from occurring (no coffee in server room)
2. **Fault Tolerance**
   - Prevent faults from becoming failures. Use redundancy to prevent (e.g. ECC error correction code).
3. **Speed Up Repair**
   - availability is improved (keep spare hard drive for when current fails)

## Fault Tolerance Techniques

1. **Checkpointing** is used for transient and intermittent faults. The state of the system is periodically saved, and when an error is detected, the system is restored to the correct state.
   - If checkpointing and system restore takes too long, then this is considered a service interruption.
2. **2-Way Redundancy** is when two modules do the same work and outcomes are compared. Roll back if the outcomes are different.
   - This method requires a system recovery technique
3. **3-way Redundancy** is when 3 or more modules do the same work and if the outcomes are different, the majority wins.
   - This method is expensive, but it can tolerate a fault in one module.

## N Module Redundancy

- N = number of modules
- N=2 Dual Module Redundancy detects but does not correct 1 faulty module
- N=3 Triple Module Redundancy corrects 1 faulty module
- N=5 Five Module Redundancy detects and corrects up to 2 modules

## Fault Tolerance For Memory and Storage

- Dual and triple module redundancy are considered overkill.
- Usually ECC codes are used and these are implemented via parity. An extra bit, called a parity bit, is added and it is XOR'ed with all of the data bits. If one of the bits flips, the result of the XOR is changed and we see that an error occurred.

## Redundant Array of Independent Disks (**RAID**)

- Several disks are used in place of one disk and each disk detects errors using ECC.
- The goal of RAID is to achieve better performance by allowing reads/writes even when there is a bad sector or when a disk fails.

### RAID 0

- Uses striping to improve performance.
- **Takes 2 disks and makes it look like 1 disk**.
- The advantages of this are:
  - x2 the data throughput of a single disk
  - Less queuing delay
- The disadvantage is that reliability is worse.
$$  MTTF_{Single\ Disk} = \frac{1}{F_{Single\ Disk}}$$
$$  Failure_{N\ Disks} = N * F_{Single\ Disk}$$
$$  MTTF_{N\ Disks} = \frac{MTTF_{Single\ Disk}}{N}$$
$$ MTT_{Data\ Loss} = MTTF_{Disk}$$

### RAID 1

- **A second disk is a copy of the first disk**.
- The write performance is the same as for 1 disk.
- The read performance is x2 throughput of 1 disk.
- Tolerates any faults that affects 1 disk.
- $$ MTT_{Data\ Loss\ RAID1} = \frac{MTTF_{Single\ Disk}}{2} + MTTF_{Single\ Disk}$$
- The above assumes that no disk is replaced. If it is replaced the reliability is:
- $$ MTT_{Data\ Loss\ RAID1} = \frac{MTTF_{Single\ Disk}}{2} * \frac{MTTF_{Single\ Disk}}{MTTR_{Single\ Disk}}\ , where\ MTTR_{Single\ Disk}\ = mean\ time\ to\ repair\ single\ disk$$

### RAID 4

- Block interleaved parity
- Uses N disks and N-1 contains data (striped like RAID 0)
- 1 disk has parity blocks
- A damaged disk that cannot be corrected with ECC can be reconstructed by using the parity bit and the data values in the other disks.
- A write to a RAID4 must write to the required disk and to the parity disk while a read just reads the required disk
- Reads get a throughput of N-1 Disks (read from all data disks but not parity disk).
- Writes get only 1/2 throughput of a single disk (a significant disadvantage and reason we need RAID 5)
- $$ MTTF_{RAID\ 4} = \frac{MTTF_{Single\ Disk} * MTTF_{Single\ Disk}}{N*(N-1) * MTTR_{Single\ Disk}}$$
- **When doing a write, the data and parity bit must both be written.**
- To update the parity bit without reading all the data bits:
  1. XOR the old data with the new data
  2. XOR this result with the parity bit
  3. The final result of this XOR is the new parity bit
- The parity disk will be a bottleneck because every read and write must access the parity bit.

### RAID 5

- Uses distributed block interleaved parity but the parity is spread amongst all of the disks.
- Read Performance = N * Throughput of one disk
- Write Performance = N/4 * Throughput of 1 disk
- Reliability is same as RAID 4

### RAID 6

- Two Parity Blocks/ Group similar to RAID 5 but with two parity bits per group
- RAID 6 can still work with two failed stripes
- The two parity blocks are different: one is a parity bit and the second is a check block. If one disk fails, use the parity bit. If two disks fail, use equations to reconstruct the data.
- The overhead is much higher than for RAID 5 and the probability of a second disk failing before the first failed disk is repaired is very small.
- RAID 6 is overkill if one assumes the disk failures are independent. However, if the failures are related it is not.
