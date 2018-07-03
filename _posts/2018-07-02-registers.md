---
layout: post
title:  "x86 Registers Overview"
date:   2018-07-02 09:53:41 +0000
categories: x86
---

# x86 Registers

The purpose of this post is to help me learn and remember the basic rules of x86 registers.
I've posted it here to benefit myself any others.
These notes are heavily inspired by [x86 Assembly Guide, University of Virginia](http://www.cs.virginia.edu/~evans/cs216/guides/x86.html)

x86 processors have eight 32-bit general purpose registers.  
32 bit (4 byte) registers EAX, EBX, ECX and EDX have subsections that can be referenced.  
The least significant 2 bytes of EAX, called AX, can be used as a 16 bit register.  
The least significant byte of AX, called AL, can be used as an 8 bit register.  
The most significant bytes of AX, called AH, can be used as an 8 bit register.   

<table>
    <colgroup>
        <col width="25%" />
        <col width="25%" />
        <col width="25%" />
        <col width="25%" />
    </colgroup>
    <thead>
        <tr class="header">
            <th>32 bit</th>
            <th>16 bit</th>
            <th>8 bit</th>
            <th>8 bit (Least Significant Byte)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="background-color:#f5eee6">EAX</td>
            <td style="background-color:#f3d7ca">AX</td>
            <td style="background-color:#e6a4b4">AH</td>
            <td style="background-color:#c86b85">AL</td>
        </tr>
        <tr>
            <td style="background-color:#f5eee6">EBX</td>
            <td style="background-color:#f3d7ca">BX</td>
            <td style="background-color:#e6a4b4">BH</td>
            <td style="background-color:#c86b85">BL</td>
        </tr>
        <tr>
            <td style="background-color:#f5eee6">ECX</td>
            <td style="background-color:#f3d7ca">CX</td>
            <td style="background-color:#e6a4b4">CH</td>
            <td style="background-color:#c86b85">CL</td>
        </tr>
        <tr>
            <td style="background-color:#f5eee6">EDX</td>
            <td style="background-color:#f3d7ca">DX</td>
            <td style="background-color:#e6a4b4">DH</td>
            <td style="background-color:#c86b85">DL</td>
        </tr>
        <tr>
            <td style="background-color:#f5eee6" colspan="4">ESI</td>
        </tr>
        <tr>
            <td style="background-color:#f5eee6" colspan="4">EDI</td>
        </tr>
        <tr>
            <td style="background-color:#f5eee6" colspan="4">ESP (Stack Pointer)</td>
        </tr>
        <tr>
            <td style="background-color:#f5eee6" colspan="4">EBP (Base Pointer)</td>
        </tr>    
    </tbody>
</table>

# Static Data Regions

Static data regions are the global variables of a software program.  
Data directives DB, DW and DD are used to declare one, two and four byte data locations.  
Data locations can be labeled with names or left anonymous.  

**Examples**

{% highlight assembly %}
.Data

var   DB  55   ; Declare a byte value, labeled as location var, containing a value of 55
var2  DB  ?    ; Declare a byte value, labeled as location var2, uninitialised
      DB  10   ; Declare a byte value, with no label, containing a value of 10, Its location is var2 + 1
X     DW  ?    ; Declare a 2 byte value, labeled as location X, uninitialised

Y     DD  1000 ; Declare a 4 byte value, labeled as location Y, containing a value of 1000

{% endhighlight %}

# Static Data Arrays

x86 arrays are defined as a number of data values listed contiguously in memory.  
An array can be declared as listing the values.
An array can be declared using the DUP directive.
An array can be declared as listing string values.  

**Examples**

{% highlight assembly %}
.Data

Z  DD  1, 2, 3    ; Declare three 4 byte values, initialised to 1, 2 and 3
T  DB  10  DUP(?) ; Declare 10 uninitialised bytes
U  DD  100  DUP(0) ; Declare 100 4 byte values, each initialised to 0
P  DB  'world', 0 ; Declare 6 bytes starting at address P, initialised to the values world and 0

{% endhighlight %}

# Addressing Memory

x86 processors can address up to 2<sup>32</sup> bytes of memory. 
Memory addresses are 32 bits wide.  
Value labels are replaced by the assembler with 32 bit quantities.  
Addressing is used by many x86 instructions.  

**Examples**

{% highlight assembly %}

mov  eax,       [ebx]       ; Move the 4 bytes in memory at the address contained in EBX into EAX.
mov  [var],     ebx         ; Move the contents of EBX into the 4 bytes at memory address var.  
mov  eax,       [esi-4]     ; Move 4 bytes at memory address ESI + (-4) into EAX.  
mov  [esi+eax], cl          ; Move the contents of CL into the byte at address ESI + EAX.   
mov  edx,       [esi+4*ebx] ; Move the 4 bytes of data at address ESI+4*EBX into EDX.  

{% endhighlight %}

# Size Directives

Generally the size of the data item at a given memory address can be inferred.  
When a 32 bit register is loaded, the assembler infers that the region of memory referred is 4 bytes wide.
When the value of a one byte register is stored, the assembler infers that the address should refer to a single byte.  
In some cases the operation is ambiguous.
The size directives BYTE PTR, WORD PTR and WORD PTR indicate 1, 2 and 4 bytes respectively.

**Examples**

{% highlight assembly %}

mov  BYTE PTR [ebx], 2  ; Move 2 into the single byte at the address stored in EBX.  
mov  WORD PTR [ebx], 2  ; Move the 2 byte integer representation of 2 into the 2 bytes starting at address stored in EBX.  
mov  DWORD PTR [ebx], 2 ; Move the 4 byte integer representation of 2 into the 4 bytes starting at address stored in EBX.  
 
{% endhighlight %}

# Data Movement Instructions

**mov** - The mov instruction copies the data item referred to by its second operand into the location referred to the first operand.  

Register to register moves are possible.
Memory to memory moves are not possible.

**Examples**

{% highlight assembly %}

mov eax, ebx          ; Copy the value in EBX to EAX.  
mov BYTE PTR [var], 5 ; Store the value of 5 into the byte at location var.  

{% endhighlight %}

**push** - The push instruction places its operand onto the top of the stack.  

Push first decrements ESP by 4, then places its operand into the contents of the location at address ESP.  
ESP, the stack pointer is decremented as the x86 stack grows down.
The stcak grows from high addresses to lower addresses.

**Examples**

{% highlight assembly %}

push eax   ; Push eax onto the stack
push [var] ; Push the 4 bytes at address var onto the stack.   

{% endhighlight %}

**pop** - The pop instruction removes the 4 byte data element from the top of the stack into the specified operand.  

Pop first moves the 4 bhtes located at memory location [SP] into the specified register or memory location and then increments SP by 4.  

**Examples**

{% highlight assembly %}

pop edi   ; Pop the top element of the stack into EDI.  
pop [ebx] ; Pop the top element of the stack into the four bytes starting at location EBX.  

{% endhighlight %}

# Arithmetic and Logic Instructions

**add** - The add instruction adds together two operands and stores the result in the first.  

Both operands may be registers.  
Only one operand can be a memory location.  

**Examples**

{% highlight assembly %}

add eax, 10            ; The value of EAX is changed to EAX + 10.  
add BYTE PTR [var], 10 ; Add 10 to the single byte stored at memory address var.  

{% endhighlight %}

**sub** - The sub instruction subtracts the value of the second operand from the value of the first operand, and stores the result in the first operand.  

**Examples**

{% highlight assembly %}

sub al, ah   ; The value of AL is changed to AL - AH.  
sub eax, 216 ; Subtract 216 from the value stored in EAX.  

{% endhighlight %}

**inc, dec** - The inc instruction increments the contents of its operand by 1.  
The dec instruction decrements the contents of its operand by 1.

**Examples**

{% highlight assembly %}

inc DWORD PTR [var] ; Increments the 4 byte value stored in var.  
dec eax             ; Decrements EAX.  

{% endhighlight %}

**and, or, xor** - These instructions perform bitwise manipulations.  

**Examples**

{% highlight assembly %}

and eax, 0fH ; Clear all but the last 4 bits of EAX.  0f hex equals b00001111  
xor edx, edx ; Sets the contents of EDX to 0.  

{% endhighlight %}

**not** - Bitwise not

**Examples**

{% highlight assembly %}

not BYTE PTR [var] ; Negates all bits in the byte at the memory location var.  

{% endhighlight %}

# Control Flow Instructions

x86 processors maintain a 32 bit value instruction pointer IP register.  
The IP increments to point to the next instruction in memory.  
Labels are used to as places in the program that the IP can be moved to.

**jmp** - The jmp instruction transfers program to the instruction at the memory indicated by the operand.  

**Example**

{% highlight assembly %}

jmp start ; Jump to the instruction labeled start.  

{% endhighlight %}

**jcondition** - The conditional jump instructions transfers the instruction pointer based on a condition.  The conditional jump instructions are used in conjunction with the cmp (compare) instruction.  

The available conditional jump instructions are:

je - jump when equal  
jne - jump when not equal  
jz - jump when the last result was zero  
jg - jump when greater than  
jge - jump when greater than or equal to
jl - jump when less than  
jle - jump when less than or equal to  

**Example**

{% highlight assembly %}

cmp eax, ebx
jle done      ; If the contents of EAX are less than or equal to the contents of EBX, jump to the label done.  

{% endhighlight %}

**call, ret** - Subroutine call and return

The call instruction first pushed the current code location onto the stack and then performs an unconditional jump to the code location indicated by the label operand.  
The ret instruction implements a subroutine return mechanism.  
This pops a code location off the stack and then performs an unconditional jump to the retrieved code location.

**Example**

{% highlight assembly %}

call doit
ret

{% endhighlight %}