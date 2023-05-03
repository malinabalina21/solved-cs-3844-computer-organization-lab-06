Download Link: https://assignmentchef.com/product/solved-cs-3844-computer-organization-lab-06
<br>



This lab focuses on the stack frame, calling a function, passing arguments, and return values. We call a recursive function “printArgV” which prints the arguments passed into the program. For this lab, enter the following list of words “bat”, “Elephant”, “LEOPARD”, “whale”, “bird” into the command arguments as shown in Figure 1 – Lab #6 Setup. You are free to create your own list but for your values to match the labs, this is the list to use.




Figure 1 – Lab #6 Setup




The “main” function has two parameters: “int argc” and “char *argv[].”  The argc parameter is the number of elements in the argv[] array. The argv[] array is an array of character pointers. So each element of the array is the address of a character – in this case, the first character in a null-terminated string. Since argv[0] is always the path and filename of the program we are running, argc is always at least one. NOTE: Since array indexes start at zero, argv[argc] is invalid.




Compile and run the program and observe what it does. It recursively prints the last argument down to argument #1, (recall, Argument #0 is the path/program name) as shown below.




<strong>Argument #5:</strong>

<strong>00982134: 62 69 72 64 00  bird</strong>




The address is the value of the argv[5] array element, the next 4 values are the hexadecimal numbers representing the four ASCII characters in the word, “bird.” Addresses may differ.




Take a few minutes to study the source code. MAKE SURE you understand what and why it does what it does because if you don’t understand the source code, the assembly is going to be worse! Trust me, you will spend more time if you skip this step. On the exam, you won’t even get source code for some questions.




Set a breakpoint on the line in main: resulti = printArgV … Also set a breakpoint on the return instruction after that. And finally, one more BP at the line inside the printArgV function: “printf(“%s”, msg);.” Set up at least one memory window with 16 bytes displayed.




Run the program inside the debugger and view disassembly. This should look very familiar since it is almost identical to HW#4. There is an extra parameter to printArgV, the message which is passed in as a literal string. Note: If you are not seeing the machine code values in the disassembly window, right-click and select “Show Code Bytes.”




<ol>

 <li>Describe what is meant by a literal string.</li>

</ol>




Notice the literal string is not pushed, but rather the address of that string. (Do NOT be confused by the word “offset” – in this situation it just means “address”)




<ol start="2">

 <li>Single-step once to execute the push instruction. Get the address of the string from the stack. Type that address into a memory window. What is the address? Cut/paste the data in the memory window here for your answer.</li>

</ol>




<ol start="3">

 <li>Step to the “call” instruction (Takes two steps because of the intermediate jmp). Put esp in the memory window. Show the top 16 bytes on the stack here.</li>

</ol>




<ol>

 <li>What is the value of 5 representing?</li>

</ol>




<ol>

 <li>What is the return address?</li>

</ol>




Notice that for the declaration “int x;” there is no assembly code. Reserving space on the stack is all that is required.




<ol start="4">

 <li>Single-step down to the jne instruction.</li>

</ol>




<em>Jump Not Equal means it will jump when the zero flag is not equal to one (i.e. Z = 0). In human terms, the jne simply means that the jump will be taken when the two things being compared are not equal to each other. If they are, then zero is the result, and Z= 1. Look at the CMP (compare) instruction and the value of argc and determine if the jump is taken or not. CMP will perform this subtraction:  argc – 0; The answer is not stored but the flags are set so Z=1 only when argc = 0.</em>




<ol>

 <li>The parameter argc is stored at what displacement from ebp?</li>

</ol>




<ol>

 <li>What is its value? Find it on your stack.</li>

</ol>




<ol>

 <li>In the debugger you can hover over it to see the value, but not on the exam. What is the value in ebp and at what address is argc stored on the stack?</li>

</ol>




<ol>

 <li>Based on that, will the jne be taken?</li>

</ol>










<ol start="5">

 <li>Single-step to the “<strong>lea edx,[ecx+eax*4]</strong>” instruction. Ecx is the base register and has a value of the address of argv. (0x662120 for my lab run, may vary). Eax is equal to argc. (5).</li>

</ol>




<ol>

 <li>What will edx equal after executing this instruction given my numbers above?</li>

</ol>




<ol>

 <li>If this were a “mov edx,[ecx+eax*4]” instruction, instead of copying the calculated value into edx, it would take the additional step of using that value as an address and copying the contents of that memory location into edx. Execute the lea instruction, type edx in the memory window, and show the contents of memory here:</li>

</ol>




<ol start="6">

 <li>Now go ahead and press the continue button (green arrow) and look at the contents of the output window:</li>

</ol>




<strong>There are 6 arguments to this program.</strong>

<strong> </strong>

<strong>Argument #5:</strong>

<strong>00662134: 62 69 72 64 00   bird</strong>

<strong> </strong>

<strong>Argument #4:</strong>

<strong>00662130: 77 68 61 6C 65 00   whale</strong>

<strong> </strong>

<strong>Argument #3:</strong>

<strong>0066212C: 4C 45 4F 50 41 52 44 00   LEOPARD</strong>

<strong> </strong>

<strong>Argument #2:</strong>

<strong>00662128: 45 6C 65 70 68 61 6E 74 00   Elephant</strong>

<strong> </strong>

<strong>Argument #1:</strong>

<strong>00662124: 62 61 74 00   bat</strong>




Your screen should look similar to the above output, though addresses may vary. In the assembly code, this time, that jne was not taken.




<ol start="98">

 <li>Show msg in a memory window. The value of msg is the address: 0x582C98. It’s contents are the ASCII characters you see.</li>

</ol>




Visual Studio is a little confusing.  It shows this:

“<strong>0052D60F 8B 45 10  mov eax,dword ptr [msg]</strong>”




in the disassembly window. However, it is actually this:

“<strong>0052D60F 8B 45 10  mov eax,dword ptr [ebp + 0x10]</strong>.”




So we are moving the value of the 3<sup>rd</sup> argument into eax which is 0x582C98. But it looks like we are using msg as a displacement like this:

“<strong>0052D60F  A1 98 2C 58 00   mov eax,dword ptr [msg (582CA1h)].</strong>”




Note how the address is directly embedded in the machine code.




<ol>

 <li>If we did this: mov edx, dword ptr [msg] (msg=0x582CA1 used as displacement) what value would be in edx?</li>

</ol>




<ol>

 <li>If we did this: lea edx, dword ptr [msg] what value would be in edx?</li>

</ol>




<ol start="7">

 <li>We are going to walk the stack. Each stack frame has 0x44 bytes for local variables, 4 bytes for the prior ebp, 4 bytes for the return address, 12 bytes for the arguments, and 12 bytes for saving registers, for 0x64 bytes total. Put ebp in the memory window.</li>

</ol>




<ol>

 <li>Show 16 bytes of the memory window.</li>

</ol>




<ol start="24">

 <li>EBP is pointing to the prior ebp, so type the address shown in the memory window and show the top 16 bytes below. Note that 0x19FCC0 + 0x64 = 0x19FD24.</li>

</ol>




<ol>

 <li>At what address is the first argument on this stack frame and what does it equal?</li>

</ol>




<ol>

 <li>Show the next stack frame here.</li>

</ol>




<ol>

 <li>What is the return address for this stack frame?</li>

</ol>













<ol start="8">

 <li>You can continue until you reach the top stack frame if you like, but otherwise stop the debugger. Put a breakpoint on the “for” loop and run the debugger again. You can use the debugger to step through this code to help (if needed). Rather than comment all of this code, I am going to ask specific questions.</li>

</ol>




<strong>0052D645 C7 45 FC 00 00 00 00  mov    dword ptr [x],0 </strong>

<strong>0052D64C EB 09            jmp         printArgV+57h (52D657h) </strong>

<strong> </strong>

<strong>0052D64E</strong><strong> 8B 45 FC         mov         eax,dword ptr [x] </strong>

<strong>0052D651 83 C0 01         add         eax,1 </strong>

<strong>0052D654 89 45 FC         mov         dword ptr [x],eax </strong>

<strong> </strong>

<strong>0052D657 8B 45 08         mov         eax,dword ptr [argc] </strong>

<strong>0052D65A 8B 4D 0C         mov         ecx,dword ptr [argv] </strong>

<strong>0052D65D 8B 14 81         mov         edx,dword ptr [ecx+eax*4] </strong>

<strong>0052D660 52               push        edx  </strong>

<strong>0052D661 E8 89 E0 FF FF   call        @ILT+1770(_strlen) (52B6EFh) </strong>

<strong>0052D666 83 C4 04         add         esp,4 </strong>

<strong> </strong>

<strong>0052D669 39 45 FC         cmp         dword ptr [x],eax </strong>

<strong>0052D66C 7F 20            jg          printArgV+8Eh (52D68Eh) </strong>

<strong> </strong>

<strong>0052D66E 8B 45 08         mov         eax,dword ptr [argc] </strong>

<strong>0052D671 8B 4D 0C         mov         ecx,dword ptr [argv] </strong>

<strong>0052D674 8B 14 81         mov         edx,dword ptr [ecx+eax*4] </strong>

<strong>0052D677 8B 45 FC         mov         eax,dword ptr [x] </strong>

<strong>0052D67A 0F BE 0C 02      movsx       ecx,byte ptr [edx+eax] </strong>

<strong>0052D67E 51               push        ecx  </strong>

<strong>0052D67F 68 74 2C 58 00   push        offset string “%02X ” (582C74h) </strong>

<strong>0052D684 E8 7A EA FF FF   call        @ILT+4350(_printf) (52C103h) </strong>

<strong>0052D689 83 C4 08         add         esp,8 </strong>

<strong> </strong>

<strong>0052D68C EB C0            jmp         printArgV+4Eh (52D64Eh)</strong>

<strong>0052D68E</strong><strong> 8B 45 08         mov         eax,dword ptr [argc] </strong>

<strong> </strong>

<strong> </strong>

<ol>

 <li>What is the instruction at 0x52D645 doing?</li>

</ol>




<ol>

 <li>What are the 3 instructions at 0x52D64E doing?</li>

</ol>




<ol>

 <li>For instructions at 0x52D657, assume that argc =3 and argv = 0x22120, from what address does edx get its value? (Hint: you can type ecx+eax*4 in the memory window.)</li>

</ol>




<ol>

 <li>If this was an array of “short int” values instead of “char *” values, how would you write this instruction “<strong>mov edx,dword ptr [ecx+eax*4]</strong>” differently?</li>

</ol>




<ol>

 <li>What are the instructions at 0x52D669/6C doing?</li>

</ol>




<ol>

 <li>Where does the value in eax come from?</li>

</ol>




<ol>

 <li>“jg” (jump if greater than) is a signed jump – why is it signed?</li>

</ol>










<ol start="9">

 <li>Argv is an array of pointers (a double pointer). Each element contains an address of a character. In this case, it is a character string. After this instruction “<strong>52D674: mov edx,dword ptr [ecx+eax*4]</strong>” edx contains the address of the character string corresponding to the argc element number.</li>

</ol>




<ol>

 <li>Describe what is eax being used to do in this instruction “<strong>movsx ecx, byte ptr [edx+eax]</strong>?”</li>

</ol>




<ol>

 <li>The scale factor is one (not shown) but why is it one?</li>

</ol>




<ol>

 <li>Describe what ecx contains after the above instruction executes?</li>

</ol>







<ol start="10">

 <li>This concludes Lab #6</li>

</ol>





