<link rel="stylesheet" href="style.css">


# Top-level view of computer system

## Definition
A computer is an electronic system that process data and instructions. At top level, it consists of:  

| module | work |
| ------ | ---- |
| CPU    | Executes instructions |
| Memory | Stores data and programs |
| IO Unit | Communicates with the outside world |

### CPU Components

| module | work |
| ------ | ---- |
| CU    | Coordinates operations, fetches and decodes instructions |
| ALU | Performs arithmetic and logic operations |
| REG | Very fast storage for operands and intermediate results |

### CPU Buses
- Connect CPU, Memory and IO.  
- Types:
  - Address Bus
  - Data Bus
  - Control Bus

## Diagram  
```graphql
            ┌───────────┐
            │   Input   │
            └─────┬─────┘
                  │
        ┌─────────▼─────────┐
        │       Memory      │
        └─────────┬─────────┘
                  │
        ┌─────────▼─────────┐
        │        CPU        │
        │  CU | ALU | REG   │
        └─────────┬─────────┘
                  │
            ┌─────▼─────┐
            │   Output  │
            └───────────┘

```

## Summary  
A computer consists of CPU, Memory, and IO interconnected by System Buses, where the CPU executes instructions using its ALU, Control Unit, and Registers.  

---

# Bus Functional Structure

## Definition  
A bus is a set of parallel lines used for communication between CPU, Memory, and IO. Functionally, it is divided into three types:  

1. Address Bus  
  - Purpose: Selects the Memory or IO location for READ / WRITE.
  - Direction: Unidirectional (CPU -> Memory, IO).
  - Notes: Width determines maximum addressable memory. 
2. Data Bus
  - Purpose: Transfer actual data or instructions.  
  - Direction: Bidirectional.
  - Notes: Width determines word size and throughput.
3. Control Bus 
  - Purpose: Carries Control and Timing signals.
  - Signals include: READ, WRITE, CLOCK, RESET, INTERRUPT, BUS REQUEST / GRANT. 
  - Direction: Both directions.  
  - Notes: Ensures correct operation and synchronization.  

## Memory READ operation flow
1. CPU places address on `Address Bus`.
2. CPU asserts READ on `Control Bus`.
3. Memory places data on `Data Bus`.
4. CPU reads data.

## Summary  
The system bus consists of the address bus for specifying locations, the data bus for transferring information, and the control bus for coordinating operations between CPU, memory, and IO devices.  

---

# Bus Structure  

## Definition  
Bus structure describes how buses are organized in a computer system to connect CPU, memory, and IO. It affects performance, scalability, and cost. 

1. Single bus structure
  - Concept: One bus connects CPU, Memory, and all IO devices.  
  - Characteristics: Only one transfer at a time, simple design.
  - Advantage: Low cost, easy to implement. 
  - Disadvantage: Low performance, bus contention, poor scalability.  
  - Use Case: Small systems, micro-controllers.

**Diagram**
```mathematica
CPU ────────────── Memory
  │
  ├────────────── I/O Device 1
  └────────────── I/O Device 2
```

2. Multiple bus structure
  - Concept: Separate buses for `CPU <-> Memory` and `CPU <-> IO`.
  - Characteristics: Parallel transfer possible, reduced contention.
  - Advantages: Higher performance, scalable.
  - Disadvantages: More complex, higher cost, arbitration needed.
  - Use Case: General-purpose computers.  
  
  **Diagram**
```mathematica
        Memory Bus
CPU ────────────── Memory

        I/O Bus
CPU ────────────── I/O Devices
```

3. Mezzanine (Hierarchical) bus structure
  - Concept: Buses arranged in layers: high-speed local bus near CPU, slower peripheral buses.  
  - Characteristics: Optimized for speed, reduces electrical load, scalable.  
  - Advantages: High performance, supports multiple levels of devices.  
  - Disadvantages: Complex design, bridges / controller needed.  
  - Use Case: Modern computers (PCIe, chip-set buses, memory channels).  

**Diagram**  

```mathematica
CPU
 │
High-speed Local Bus
 │
Bridge
 │
Peripheral Bus
 │
I/O Devices
```

## Summary  
Bus structures range from single-bus(simple, low-performance) to multiple and mezzanine buses(higher performance and scalability through separation and hierarchy).  

---

# CPU Registers

## Definition  
Registers are high-speed storage locations inside the CPU used to hold operands, instructions, addresses, or intermediate results during instruction execution.  

### Types of Registers
#### General Purpose Registers (GPRs)
  - Hold operands, results, and temporary data.  
  - Directly accessible by instructions.  
  - Examples: `R0->R31` (ARM / RISC-V), `EAX / EBX` (x86).  
  - Help reduce memory access -> improves performance.  
#### Special Purpose Registers  

| Register | Purpose |
| -------- | ------- |
| Program Counter | Holds address of next instruction |
| Instruction Register | Holds current instruction |
| Stack Pointer | Points to top of stack |
| Flag Register | Holds condition flags (zero, carry, overflow) |
| Control Register | Configure CPU modes, interrupt |

#### Memory Access Registers
Interface CPU with Memory.  

| Register | Purpose |
| -------- | ------- |
| Memory Address Register | Holds memory location to READ / WRITE |
| Memory Buffer Register | Holds data being transferred to / from memory |

## How Register fit in Fetch-Execute cycle  
- MAR & MBR -> Move data to / from memory.  
- IR & PC -> Fetch and sequence instructions.  
- GPTs & ALU -> Performs computations.  

## Summary
Registers are the CPU's fastest storage, classified as general-purpose, special-purpose, and memory-access, and they directly support instruction fetch, execution, and memory operations.  

---

# Instruction Format (Opcode & Operands)  

## Definition
An instruction is a binary pattern that tell the CPU what operation to perform and on what data.

It has two main elements:

### Opcode
- Specifies the operation to perform.  
- Examples: `LOAD`, `ADD`, `STORE`, `JMP`.  
- Decoded by the control unit to generate control signals.  

### Operands
- Specify the data or the location of the data.  
- Can be:
  - Registers.  
  - Memory addresses.  
  - Immediate values.  
  - IO locations.  
- Number of operands varies: 0, 1, 2, or 3.  

## Example instructions  

 | Instruction | Opcode | Operands | Meaning |  
 | ----------- | ------ | -------- | ------- |
 | ADD R1, R2, R3 | `ADD` | `R1`,`R2`,`R3` | R1<-R2+R3 |
 | LOAD R1, 0x1000 | `LOAD` | `R1`, `0x1000` | R1 ← Memory[0x1000] |

 ## Summary
 An instruction consists of an opcode, which specifies the operation, and operands which specify the data or location involved in the operation.  

 ---

 # Instruction Cycle (Fetch-Decode-Execution-Write Back)

 ## Definition
 The instruction cycle is the repeating sequence of steps of the CPU follows to executed a program, instruction by instruction.  

 ### Steps of the cycle
 #### Fetch
 - Get the instruction from memory
 - Steps:
  - PC -> MAR
  - Control bus: Memory READ
  - Memory -> MDR -> IR
  - PC <- PC + 1

#### Decode
- Control unit decodes opcode in IR
- Determines:
  - Instruction type (ALU, Memory, Branch)
  - Operand location
  - Addressing mode
- Generates control signals

#### Execute
- Perform the operation
  - ALU instructions: Read registers -> ALU -> WRITE result to register.  
  - LOAD / STORE: Compute address -> memory transfer
  - BRANCH / JUMP: Evaluate condition -> update PC if needed.

#### Write Back
- Results of execution written to registers or memory.  
- Flags updated.  

## Diagram

```graphql
        ┌─────────┐
        │  Fetch  │
        └─────┬───┘
              │
        ┌─────▼───┐
        │ Decode  │
        └─────┬───┘
              │
        ┌─────▼────┐
        │ Execute  │
        └─────┬────┘
              │
        ┌─────▼──────┐
        │ Write-Back │
        └────────────┘
```

## Key notes  
- PC is incremented during fetch, not after execution.  
- Every instruction passes through fetch and decode.
- Execution depends on instruction type: write-back may be optional.

## Summary
The instruction cycle fetches the instruction from memory, decodes it to generate control signals, executes it using the ALU or memory, and writes the result back to registers or memory, repeating for all instructions.  

---

# Expanded Instruction Cycle (IF, IOD, OAC, IAC)  

## Definition
The expanded instruction cycle breaks the basic fetch–decode–execute cycle into finer states to handle different instruction types and addressing modes.  

## States
### IF — Instruction Fetch
- Fetch the next instruction from memory
- Steps:
  - PC → MAR
  - Memory Read → MDR → IR
  - PC ← PC + 1
- Mandatory for all instructions

### IOD — Instruction Opcode Decode
- Decode the instruction in IR
- Determine:
  - Operation type
  - Number of operands
  - Addressing mode
- Control signals are generated
- Mandatory for all instructions

### OAC — Operand Address Calculation
- Compute the effective address if the instruction accesses memory.
- Steps:
  - Base register + offset or index
  - Result → MAR
- Conditional: Only for memory-access instructions (LOAD, STORE)

### IAC — Instruction Address Calculation
- Compute target address for branch, jump, or call instructions
- Evaluate condition if necessary
- Update PC if branch taken
- Conditional: Only for control transfer instructions

## Diagram
```graphql
          ┌─────────┐
          │    IF   │
          └─────┬───┘
                │
          ┌─────▼───┐
          │   IOD   │
          └─────┬───┘
   ┌────────────┼────────────┐
   ▼            ▼            ▼
  OAC           IAC         Execute
(memory)   (branch/jump)      │
                └─────────────┘
```

## Summary
In the expanded instruction cycle, the CPU moves through Instruction Fetch (IF), Instruction Opcode Decode (IOD), Operand Address Calculation (OAC), for memory instructions, and instruction Address Calculation (IAC) for branch instructions, with conditional transitions based on instruction type.  

---

# Interrupt Cycle

## Definition

An interrupt is a signal that causes the CPU to temporarily suspend the current program and execute an Interrupt Service Routine (ISR).  

## Key rule (do not forget)
Interrupts are checked after completion of an instruction, never in the middle.

## Steps in the Interrupt Cycle
1. Interrupt Check: CPU checks if an interrupt is pending and enabled.
2. Save Context: Current PC is saved on stack. Flags are saved.
3. Identify Interrupt: CPU determines which interrupt occurred.
4. Load ISR Address: PC <- address of ISR.
5. Execute ISR: Device or event is serviced, Interrupt flag cleared.
6. Return from Interrupt: PC, Flags restored.

## Diagram

```mathematica
Execute Instruction
        ↓
 Check Interrupt?
        ↓
 Save Context
        ↓
 Execute ISR
        ↓
 Restore Context
        ↓
 Continue Program
 ```

 ## Interrupt Types

 ### By Source
 #### Hardware Interrupts
 - Generated by external devices.
 - Examples: Keyboard, disk, timer.
 - Asynchronous.

 #### Software Interrupts
 - Generated by instructions.
 - Examples: System Calls, Traps.
 - Synchronous.

 ### By Masking
 #### Maskable Interrupts
 - Can be enabled or disabled by CPU.
 - Used for normal IO.
 #### Non-Maskable Interrupts
 - Can't be disabled.
 - Used for critical events (power failure).

 ### By Timing
 #### Synchronous Interrupts
 - Caused by instruction execution.
 - Example: Divide By Zero.
 #### Asynchronous Interrupts
 - Independent of program flow.
 - Example: IO completion.

 ### By Vectoring
 #### Vectored Interrupts
 - ISR address predefined.
 - Faster handling.
 #### Non-Vectored Interrupts
 - ISR address determined by software.
 - Slower.

 ## Summary
 Interrupts allow the CPU to respond efficiently to internal and external events by saving the current context, executing an interrupt service routine, and then resuming normal execution.

---

# Von Neumann Bottleneck 

## Definition
The Von Neumann bottleneck is the performance limitation caused by a computer architecture where instructions and data share the same memory and the same bus.

## Cause
- Single memory stores both instructions and data.
- Single data path (Bus) connects CPU and Memory.
- CPU can either fetch an instruction or access data, not both at the same time.

## Effects
- CPU stalls while waiting for memory.
- Performance becomes memory-bound, not CPU-bound.
- Increasing CPU speed alone does not significantly improve performance.

## Relation to Instruction Cycle
- Instruction fetch and data access compete for the same bus.
- Memory latency directly affects CPI

## Mitigation Techniques
- Cache memory (L1, L2, L3) to exploit locality.
- Instruction prefetching.
- Pipelining to overlap operation.
- Modified Harvard architecture.

## Summary
The Von Neumann bottleneck arises from a shared instruction and data memory path, limiting system performance due to memory bandwidth and latency constraints.

---

# Instruction Set Design: RISC and CISC

## Definition
Instruction set design defines how instructions are structured and executed, balancing complexity between hardware and software.

## RISC: Reduced Instruction Set Computer
### Design Philosophy
Keep instruction simple and uniform so hardware can execute them very fast.

### Characteristics
- Small instruction set
- Fixed-length instruction
- Few addressing modes
- Load/Store architecture
- Most instructions execute in one cycle

### Example
```css
LOAD  R1, A
LOAD  R2, B
ADD   R3, R1, R2
STORE R3, A
```

## CISC: Complex Instruction Set Computer
### Design Philosophy
Use powerful instruction to reduce program size.

### Characteristics
- Large instruction set
- Variable-length instructions
- Many addressing modes
- Memory-to-memory operations allowed
- Instructions may take multiple cycles

### Example
```css
ADD A, B
```

## Comparison table

| Aspect | RISC | CISC |
| ------ | ---- | ---- |
| Instruction complexity | Simple | Complex |
| Instruction length | Fixed | Variable |
| Addressing modes | Few | Many |
| Memory access | LOAD / STORE | Direct memory operations |
| Cycles per instruction | Usually 1 | Variable |
| Control unit | Hardwired | Microprogrammed |

## Modern Reality
- Modern CPUs are hybrids.
- x86 (CISC) uses RISC-like micro-operation internally.
- Performance today depends more on cache and parallelism than ISA style. 

## Summary
- RISC and CISC differ in instruction set philosophy, with RISC emphasizing simplicity and speed, and CISC emphasizing expressive instructions and compact programs.

---

# Possible math

## Average CPI

CPU<sub>avg</sub> = SUM ( CPI<sub>i</sub> * Instruction Fraction<sub>i</sub> )

## Cache Miss Penalty

Memory Stall CPI = Miss Rate * Miss Penalty    
Total CPI = Base CPI + Memory Stall CPI

## CPU Time

CPU Time = Instruction Count * CPI * Clock Cycle Time

# ALU

## ADDER

```graphql
        ⌜------------⌝  
  ⌜-----| B REGISTER | 
  |     ⌞------------⌟ 
  |           |      
  |           |       
  |           ↓      
  |     ⌜--------------⌝ 
  |     | Complimenter |   
  |     ⌞--------------⌟     
  |           |           
  |           |                          ----------------⌝ 
  |           ↓                          ↓               |
  |     ⌜----------⌝              ⌜------------⌝         |
  ⌞----→|  Switch  |              | A REGISTER |         |
        ⌞----------⌟              ⌞------------⌟         |
              |                         |                |       
              |       ⌜-----------------⌟                |
              ↓       ↓                                  |
            ⌜-----------⌝                                |
            |   ADDER   |                                |    
            ⌞-----------⌟                                |
                  |                                      |   
                  ⌞--------------------------------------⌟ 
```

