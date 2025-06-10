# Week 9: Software Reverse Engineering (SRE)

## Introduction to Software Reverse Engineering

### What is SRE?
**Software Reverse Engineering (SRE)** is the process of analyzing software to understand its functionality, structure, and behavior without having access to source code. It's also known as:
- **Reverse Code Engineering (RCE)**
- **"Reversing"**

### SRE Applications

#### Legitimate Uses (Good):
- **Malware analysis**: Understanding how malicious software works
- **Legacy code analysis**: Reverse engineering old software for maintenance
- **Security research**: Finding vulnerabilities to improve security
- **Interoperability**: Understanding proprietary protocols and formats

#### Malicious Uses (Not-so-good):
- **Software piracy**: Removing usage restrictions and licensing checks
- **Exploit development**: Finding and exploiting software vulnerabilities
- **Game cheating**: Modifying game behavior for unfair advantages
- **IP theft**: Stealing proprietary algorithms and trade secrets

## SRE Assumptions and Methodology

### Attack Model
The typical SRE scenario assumes:
- **Reverse engineer is an attacker**
- **Attacker only has the executable** (no source code access)
- **Attacker's objectives**:
  - Understand the software's functionality
  - Modify the software's behavior

### Required Skills for SRE
- **Assembly language knowledge**: Working knowledge of target architecture
- **Tool expertise**: Proficiency with sophisticated reverse engineering tools
- **File format understanding**: Knowledge of executable formats (PE for Windows, ELF for Unix/Linux)
- **Patience and persistence**: SRE is a tedious and labor-intensive process

## SRE Tools and Techniques

### Essential Tools

#### Disassemblers
- **IDA Pro**: Top-rated disassembler (costs hundreds of dollars)
  - Converts binary to assembly code
  - Provides sophisticated analysis capabilities
- **Other disassemblers**: Various free and commercial alternatives

#### Debuggers
- **SoftICE**: "Alpha and omega" of debuggers (costs thousands)
  - Kernel mode debugger
  - Can debug anything, including the OS
- **OllyDbg**: High-quality shareware debugger
  - Includes good disassembler
  - Popular among reverse engineers

#### Hex Editors
- **UltraEdit**: Good freeware option
- **HIEW**: Useful for patching executables
- Purpose: View and modify binary files directly

#### Additional Tools
- **Strace, GDD**: Various specialized analysis tools

### Practical SRE Example: Serial Number Analysis

#### The Problem
A program requires a serial number, but the reverse engineer doesn't know it.

#### The Process
1. **Disassembly with IDA Pro**:
   - Analyze the assembly code
   - Look for string references
   - Find the serial number comparison logic

2. **Discovery**:
   - Through analysis, find that the expected serial number is "S123N456"
   - Understand the test instruction: `test eax,eax`
     - Gives AND of eax with itself
     - Result is 0 only if eax is 0
     - If test returns 0, then jz (jump if zero) is true

3. **Patching**:
   - Goal: Make jz always true (bypass serial check)
   - Use hex editor to modify the executable
   - Change `test eax,eax` to `xor eax,eax` (always sets eax to 0)
   - Save as modified executable (e.g., serialPatch.exe)

## SRE Attack Mitigation Techniques

### Overview
While it's **impossible to prevent SRE on open systems**, various techniques can make attacks more difficult:

### 1. Anti-Disassembly Techniques
**Goal**: Confuse static analysis of code

#### Methods:
- **Encrypted object code**: Code is encrypted and decrypted at runtime
  - Problem: Still need code to decrypt the code (same issue as polymorphic viruses)
- **False disassembly**: Insert junk instructions to confuse disassemblers
  - Example: `inst1 jmp inst3 inst4` where disassembler sees `inst1 inst2 inst3 inst4`
- **Self-modifying code**: Code that changes itself during execution

### 2. Anti-Debugging Techniques
**Goal**: Confuse dynamic analysis of code

#### Detection Methods:
- **Monitor for debug registers**: Detect hardware breakpoints
- **Check for inserted breakpoints**: Look for software breakpoints (0xCC)
- **Thread interaction analysis**: Debuggers don't handle threads well
- **Timing analysis**: Detect if execution is slower due to debugging

#### Advanced Techniques:
- **Hardware-based debugging detection**: Monitor for HardICE and similar tools
- **Undetectable debugging**: Advanced techniques exist but are complex

### 3. Tamper-Resistance
**Goal**: Make patching more difficult

#### Implementation:
- **Self-hashing**: Code hashes parts of itself
- **Integrity checking**: If tampering detected, hash check fails
- **Guard distribution**: Spread checks throughout code
- **Variation**: Make checks look different to avoid easy removal

#### Benefits:
- Research shows good code coverage with small performance penalty
- Increases attacker workload significantly

### 4. Code Obfuscation
**Goal**: Make code harder to understand

#### Basic Techniques:
- **Spaghetti code**: Intentionally complex control flow
- **Opaque predicates**: Always-false conditionals with dead code
  - Example: `if((x-y)(x-y) > (x*x-2*x*y+y*y)){...}`
  - The condition is always false, but attackers waste time analyzing

#### Advanced Obfuscation Research:
Much academic research focuses on more robust obfuscation methods.

## Advanced Obfuscation Techniques

### Malware Evolution and Obfuscation

#### Historical Progression:
1. **Encrypted Malware**: Basic encryption with constant decryptor
2. **Oligomorphic Malware**: Few different decryptors
3. **Polymorphic Malware**: Many different decryptors, same body
4. **Metamorphic Malware**: Entire code changes between generations

### Specific Obfuscation Methods

#### 1. Dead-Code Insertion
- **Technique**: Add ineffective instructions that don't change behavior
- **Purpose**: Increase code size and analysis time
- **Detection**: Can be defeated by removing ineffective instructions

#### 2. Register Reassignment
- **Technique**: Use different registers for same operations across generations
- **Example**: EAX, EBX, EDX used interchangeably
- **Impact**: Changes instruction patterns while maintaining functionality

#### 3. Subroutine Reordering
- **Technique**: Change order of subroutines in program
- **Complexity**: Can generate factorial combinations (10! = 3,628,800 variations)
- **Limitation**: Doesn't change individual subroutine content

#### 4. Instruction Substitution
- **Technique**: Replace instructions with equivalent ones
- **Examples**: 
  - `xor` → `sub`
  - `mov` → `push/pop`
- **Effectiveness**: Changes opcodes while preserving semantics

#### 5. Code Transposition
- **Method 1**: Random shuffling with unconditional branches
  - Easily defeated by removing jumps
- **Method 2**: Reordering independent instructions
  - More complex but harder to detect

#### 6. Code Integration
- **Advanced technique**: Malware integrates itself into target program
- **Example**: Win95/Zmist virus
- **Process**: 
  - Decompile target program
  - Insert malware code between objects
  - Reassemble integrated code
- **Difficulty**: Extremely hard to detect and remove

### Theoretical Limitations

#### The Impossibility of Perfect Obfuscation
Research has shown that **perfect obfuscation is theoretically impossible**:

- **Main result**: Even under weak formalizations, general obfuscation cannot be achieved
- **Proof method**: Constructing unobfuscatable function families
- **Implication**: There will always be properties that can be efficiently computed from obfuscated code but not from black-box access

## Modern Applications and Trends

### Packer Technology
- **Purpose**: Originally for compression, now used for evasion
- **Effectiveness**: Can evade most antivirus scanners
- **Prevalence**: 48-92% of malware uses packing (varies by study)
- **Detection**: Entropy analysis and signature-based methods

### Platform-Specific Obfuscation

#### Web Malware:
- **JavaScript obfuscation**: Targeting browser vulnerabilities
- **URL encoding**: Hiding malicious URLs
- **Dynamic generation**: Runtime code construction

#### Mobile Malware:
- **Resource constraints**: Energy and performance considerations
- **Platform adaptation**: iOS and Android specific techniques
- **Evolution**: Rapidly developing field

### Virtual Machine-Based Obfuscation
- **Concept**: Create custom virtual processors
- **Implementation**: Malware runs on unknown instruction sets
- **Analysis burden**: Reverse engineers must understand both VM and code
- **Effectiveness**: Extremely time-consuming to analyze

## Commercial Tools and Examples

### JSCrambler
- **Purpose**: JavaScript application protection
- **Features**:
  - Self-defensive code
  - Tamper detection
  - Debug activity detection
  - Automatic code derailment when compromised

### Professional Applications
- **Software protection**: Legitimate use for intellectual property
- **License enforcement**: Preventing unauthorized use
- **Anti-tampering**: Protecting against modification

## Defense Strategies and Countermeasures

### Combined Approach
Effective protection typically combines multiple techniques:
- **Anti-disassembly** + **Anti-debugging** + **Tamper-resistance** + **Obfuscation**

### Limitations
- **Determined attackers**: Will ultimately succeed given enough time and resources
- **Performance impact**: Security measures can slow down legitimate use
- **Complexity**: More protection often means more potential failure points

### Realistic Expectations
- **Goal**: Increase attack cost and time, not prevent all attacks
- **Trade-offs**: Balance security, performance, and usability
- **Evolution**: Continuous arms race between attackers and defenders

## Key Takeaways

1. **SRE is inevitable**: On open systems, determined attackers can reverse engineer software
2. **Multiple layers**: Use combination of protection techniques for best results
3. **Cost vs. benefit**: Make reverse engineering more expensive than the value gained
4. **Theoretical limits**: Perfect obfuscation is mathematically impossible
5. **Practical approach**: Focus on raising the bar, not building impenetrable walls
6. **Tool proficiency**: Both attackers and defenders need sophisticated tool knowledge
7. **Ongoing evolution**: Techniques constantly evolve on both sides

## Conclusion

Software Reverse Engineering represents a fundamental challenge in cybersecurity. While perfect protection is theoretically impossible, a well-designed combination of anti-disassembly, anti-debugging, tamper-resistance, and obfuscation techniques can significantly increase the cost and complexity of reverse engineering attacks. The field continues to evolve rapidly, with new techniques emerging for both attack and defense, making it a critical area of study for cybersecurity professionals.