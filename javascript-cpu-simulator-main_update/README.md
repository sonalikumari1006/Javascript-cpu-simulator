
### Original GitHub Repository:
[GITHUB] (https://github.com/msfwebdude/javascript-cpu-simulator)

## Objective:
#### Implement a instruction in the JavaScript CPU Simulation. I have added "INC" instruction.
## Description: 
#### INC instruction increments the value of the accumulator by 1.

#### Here is the website link where I found information about the instructions:
[6502 Instruction Set] (https://www.masswerk.at/6502/6502_instruction_set.html)

## Implementation Details:
1. INC Absolute:
* Opcode: 0xE6
* Operation: `memoryArray[oneAndTwo] = memoryArray[oneAndTwo] + 1`
* Flag affected: Zero,Negative,Overflow
* Program Counter increment: 3
2. INC Null
*  Opcode: 0x1A
* Flag  affected: Zero,Negative,Overflow
* Operation: `registers.A = registers.A+1`
* Program Counter increment: 1

## Code Snippets:
```javascript
case 'INC':
    console.log("INC operandType:" ,operandType);
    switch (operandType) {
        case operandTypes.ABSOLUTE:
            this.cpuData.memoryArray[nextMemoryIndex] = parseInt(0xE6);
            nextMemoryIndex++;
            this.cpuData.memoryArray[nextMemoryIndex] = parseInt(operandValue[0]);
            nextMemoryIndex++;
            this.cpuData.memoryArray[nextMemoryIndex] = parseInt(operandValue[1]);
            nextMemoryIndex++;
            this.cpuData.assembler.privatePointer += 3;
            break;
        case operandTypes.NULL:
            this.cpuData.memoryArray[nextMemoryIndex] = parseInt(0x1A);
            nextMemoryIndex++;
            this.cpuData.assembler.privatePointer += 1;
            break;
        default:
            console.log("INC instruction not supported for operand type", operandType);
            break;  
    }
    break;
case 0xE6:
    self.loader.innerHTML = `${currentPfx} INC ${fmtAddress}`;
    var sum = this.cpuData.memoryArray[oneAndTwo] + 1;
    var overflow = (sum & 0x100) !== 0; // 0x100= 9 bits
    this.cpuData.memoryArray[oneAndTwo] = sum & 0xFF;  // 0xFF= 8  bits
    this.cpuData.flags.zero     = (sum & 0xFF) == 0;
    this.cpuData.flags.negative = (sum & 0xFF) > 0x7F;
    this.cpuData.flags.overflow=overflow;
    this.cpuData.programCounter += 3;
    break;
case 0x1A: // INC (NULL)
    self.loader.innerHTML = `${currentPfx} INC`;
    var sum = this.cpuData.registers.A + 1;
    var overflow = (sum & 0x100) !== 0; 
    this.cpuData.flags.zero     = (sum == 0);
    this.cpuData.flags.negative = (sum > 0x7F);
    this.cpuData.flags.overflow=overflow;
    this.cpuData.registers.A = sum;
    this.cpuData.programCounter += 1;
    break;
``` 
## Testing 
#### To test the "INC" instruction, add them to the assembly program and observe the changes in the "current operation" and "register"

### Here is the code assembled for testing the 'INC' instruction:
``` 
INC $0040
``` 


