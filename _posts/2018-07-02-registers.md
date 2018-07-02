---
layout: post
title:  "x86 Registers"
date:   2018-07-02 09:53:41 +0000
categories: x86
---

# x86 Registers

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


# Addressing Memory


# Size Directives


# Instructions

[Inspired by x86 Assembly Guide, University of Virginia](http://www.cs.virginia.edu/~evans/cs216/guides/x86.html)