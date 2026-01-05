# School Project N°1: ASM and Processor Architecture (Subject: Computer Systems)

This practical assignment consists of analyzing and extending a microarchitecture designed using the `Logisim` simulator. The goals are to code simple programs in assembly language, modify parts of the architecture, and design new instructions.

The simulator can be downloaded from the website http://www.cburch.com/logisim/ or from the Ubuntu repositories. It requires Java 1.5 or higher. To run it, enter the following command in a console:

`java -jar logisim.jar`

<img width="657" height="273" alt="image" src="https://github.com/user-attachments/assets/70a17fb8-681a-4c24-952e-e72fb482399b" />

To download the OrgaSmall Architecture, go to: https://github.com/fokerman/microOrgaSmall/ The version we will be using is under the name OrgaSmallWithStack.

### Assemble and Execute - Write the following file, compile it, and load it into the machine's memory:
![image](https://github.com/user-attachments/assets/a830e215-fe1b-4c8c-98d1-dc9a3495dcd5)

a) Before running the program, describe its expected behavior in words. Do not explain it instruction by instruction; the goal is to understand what the program does and what result it generates. 

`Solution`: The program compares the values of two registers, incrementing the first by +1 until both reach the same value (0x70). While the equality condition is not met, the program—using the stack and other registers—stores decreasing values into memory addresses corresponding to the incrementing register, starting from 0xAF (at address 0x50) and ending with 0x90 (at address 0x6F). When both compared registers reach the same value, the JZ (Jump if Zero) instruction executes because the Z flag is set. It jumps to the memory space labeled fin. Since the values at fin are 0xA0 and 0x00, a JMP instruction is executed (opcode: 10100) back to memory address 0x00, creating a loop.

b) Identify the memory address for each of the program's labels. 

`Solution`:

main: [0x00]
aca: [0x0a]
coso2: [0x14]
fin: [0x20]
halt: [0x22]

c) Execute the program and identify, if possible, how many clock cycles are required for the program to reach the JMP halt instruction. 

`Solution`: It is not possible to reach the halt instruction because, as explained in point "a", the program enters an infinite loop before reaching it.

d) How many microinstructions are required to execute the ADD instruction? How many for JZ? How many for JMP? 

`Solution`:

ADD: 4 microinstructions
JZ: 2 microinstructions
JMP: 1 microinstruction

e) Does the program use the stack? What data is stored in the stack? 

`Solution`: Yes. The initial value of register R0 (which was set to 0x00) is stored in the stack. The register value is pushed onto the stack (using the PUSH instruction); after performing operations and changing its value, the POP instruction is executed to restore the previous value to the register.

f) Describe in detail the operation of the PUSH, POP, CALL, and RET instructions. 

`Solution`:

PUSH |Rx|, Ry: Stores the value of Ry at the memory address pointed to by Rx (which is used as the Stack Pointer, usually R7 by convention). It then subtracts 0x01 from the value of Rx, making it point to the previous memory address, which becomes the new top of the stack.

POP |Rx|, Ry: Retrieves the last value stored in the stack (the one immediately below the current pointer) and overwrites Ry with it. It also increments the value of the register used as the pointer by 1, moving it to the next memory address.

CALL |Rx|, ...

...Ry: The current value of the PC (the address of the next instruction) is pushed onto the stack at the address pointed to by Rx. The pointer is moved back one position, and the value of Ry is loaded into the PC.

...M: Same as above, but uses an immediate value M instead of the register Ry.

RET |Rx|: The opposite of CALL. It assigns the value stored in memory at the position pointed to by Rx to the PC and increments the Rx pointer.

### Extending the Machine - Adding the Following New Instructions:
<img width="804" alt="Untitled1" src="https://github.com/user-attachments/assets/e8fcbe6d-ea5d-43ae-b773-6b61a4398b0b">

`Solution`:
The solution for this exercise is included in the ADDINMEM.zip file, which contains the following four files:
- `ADDINMEM.ops`: Independent implementation of the ADDINMEM microinstruction.
- `microOps_v2.ops`: Implementation of the ADDINMEM microinstruction integrated with the rest of the processor's microinstructions.
- `microOps_v2.mem`: Memory file containing the processor instructions (including ADDINMEM), which must be loaded into the Control Unit memory for operation in Logisim.
- `assembler_v2.py`: Modified assembler.py file that includes the new microinstruction.

### Programming - Write the following instructions in ASM:
<img width="936" alt="Untitled" src="https://github.com/user-attachments/assets/1f14b9c5-eba5-42ca-8559-e5a50e4ddbaa">

`Solution`:
The solution for this exercise is included in the processArray.zip file, which contains the following three files:
- `processArray.asm`: The processArray function written in assembly language, using the instructions supported by the processor.
- `processArray.mem`: A memory file containing the encoded instructions for the function, which must be loaded into the RAM for the program to run.
- `processArray.txt`: A text file containing the program's assembly instructions along with their corresponding memory addresses.
