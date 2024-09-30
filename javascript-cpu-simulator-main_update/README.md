
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
.org $FFFE
.word $0003  ; Set reset vector to start

.org $0003

start:
    LDX #$FF      ; Load X with 255
    TXS           ; Transfer X to Stack Pointer
    SEC           ; Set carry flag
    CLC           ; Clear carry flag

    ; Test Case 1: Increment in Absolute Mode
    LDA #$05      ; Load accumulator with 5
    STA $0040     ; Store it in memory location $0040
    INC $0040     ; Increment value at $0040
    ; Check if $0040 is now 6 and zero flag is false
    LDA $0040
    CMP #$06      ; Should be 6
    BEQ pass1     ; If equal, test passed
    JMP fail      ; If not equal, test failed

pass1:
    ; Test Case 2: Increment Leading to Zero Flag
    LDA #$FF      ; Load accumulator with 255
    STA $2000     ; Store it in memory location $2000
    INC $2000     ; Increment value at $2000
    ; Check if $2000 is now 0 and zero flag is true
    LDA $2000
    CMP #$00      ; Should be 0
    BEQ pass2     ; If equal, test passed
    JMP fail      ; If not equal, test failed

pass2:
    ; Test Case 3: Increment with Overflow Condition
    ; Log current status
    LDA $0040
    ; Dummy operation
    NOP
    NOP
    LDA #$7F      ; Load accumulator with 127
    STA $3000     ; Store it in memory location $3000
    INC $3000     ; Increment value at $3000
    ; Check if $3000 is now 128 and carry flag is false
    LDA $3000
    CMP #$80      ; Should be 128
    BEQ pass3     ; If equal, test passed
    JMP fail      ; If not equal, test failed

pass3:
    ; Test Case 4: Increment in "Null Mode"
    LDA #$03      ; Load accumulator with 3
    STA $4000     ; Store it in memory location $4000
    LDA $4000
    ORA #$00      ; Dummy operation to maintain flags
    INC $4000     ; Increment value at $4000
    ; Check if $4000 is now 4 without unexpected flag changes
    LDA $4000
    CMP #$04      ; Should be 4
    BEQ pass4     ; If equal, test passed
    JMP fail      ; If not equal, test failed
pass4:
    ; Test Case 5: Increment Leading to Negative Flag
    LDA #$7F      ; Load accumulator with 127
    STA $4000     ; Store it in memory location $4000
    INC $4000     ; Increment value at $4000
    ; Check if $4000 is now 128 and negative flag is true
    LDA $4000
    CMP #$80      ; Should be 128
    BEQ success   ; If equal, all tests passed
    JMP fail      ; If not equal, test failed

success:
    LDA #$05      ; Set success for Test Case 5
    STA $5004
    BRK            ; Break to stop execution
fail:
    LDA #$FF      ; Load accumulator with 255 to indicate failure
    STA $5000     ; Store failure indicator at memory location $5000
    BRK           ; Break to stop execution

.org $0040
.byte $00        ; Initial value for the first test

.org $2000
.byte $00        ; Initial value for the second test

.org $3000
.byte $00        ; Initial value for the third test

.org $4000
.byte $00        ; Initial value for the fourth test

``` 


