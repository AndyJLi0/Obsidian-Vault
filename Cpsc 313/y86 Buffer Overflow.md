*This note is for the 2024-01-17 lecture for CPSC 313*

We can exploit the fact that you can (sometimes) write outside of a buffer, thus corrupting memory around it.

First we write an infinite loop:
```apl
.pos 0x1000
exploit:
	nop
	andq %rax, %rax
	je exploit
```
 
```
Or something like:
```apl
.pos 0x1000
exploit:
	nop
	jmp exploit
```

Here is an example of strcopy in C:
```apl
# This function DOES WHAT?
# Input %rdi
# Input %rsi
# Output 0

# THIS IS ALL PART OF THE DRIVER PROGRAM
# Initialize the stack
irmovq  0x5000, %rsp

# Driver program to test mystery
# Note that we pass parameters to functions
# in registers %rdi and %rsi (in order)
irmovq  a, %rdi
irmovq  b, %rsi
call copy
halt

a:      .quad 0xDEADBEEF
        .quad 0

b:      .quad 0
        .quad 0
# END OF DRIVER PROGRAM

.pos 0x2000
# Copies the values in first parameter, 
# into its second parameter.

copy:  #called liked copy(x, y)
	irmovq	8, %r10		# the constant 8
loop:
	mrmovq	0(%rdi), %rax	# %rax = *x
	rmmovq	%rax, 0(%rsi)	# y = %rax
	andq	%rax, %rax	    # set the condition codes
	je	done                # if %rax is == 0, jump to done.

	addq	%r10, %rdi      # x += 8
	addq	%r10, %rsi	    # y += 8
	jmp	loop
done:
	ret
```

Here we have an attack that utilizes the stack pointer to change the return address and return to something else other than caller:
```apl
# Attack 1: A benign caller invokes a function
# who is malicious; upon return the processor
# will be in an infinite loop.

# Initialize the stack
	irmovq	0x2000, %rsp

main:
	call evil	# call function
	halt		# If we return
			# successfully, halt

# Return address is just above the stack pointer:
evil:
    # WRITE YOUR CODE HERE
    irmovq exploit %r8
	rmmovq %r8, 0(%rsp)
	ret

# Place the code you wrote in Step 1 here
exploit:
    nop
    jmp exploit

# We put this here so you can observe the contents
# of the stack (why is this at 0x1FF0 and not 0x2000?)
.pos 0x1FF0
.quad 0x0
.quad 0x0
```

### The Final Attack
We create a buffer 0x20 bytes long (4 quads). As we do our strcopy function, since our evil data is longer than 4 bytes, we set our 5th byte to contain address of our infinite loop. This value is written outside our buffer and replaces the return address, and so instead of returning to where we are suppose to, we end up at exploit!

```apl
# Attack 2: A malicious caller invokes a function
# that does not check the sizes of its
# arguments, thus producing a vulnerability.

# Initialize the stack
	irmovq	0x2000, %rsp

main:
	# PART 4
	 irmovq evildata, %rdi	# Place evil data in the first parameter.
	 call	innocent	# call function

	# Let's start by calling innocent with a friendly input
	# irmovq	gooddata, %rdi	# Place good data in the first parameter.
	#call	innocent	# call function

	#irmovq	baddata, %rdi	# Setup for call with baddata
	#call	innocent	# call function

	halt		# If we return successfully, halt

# Attack 1: A benign caller invokes a function
# who is malicious; upon return the processor
# will be in an infinite loop.

# This is valid data that won't cause a problem
# in our example
.pos 0x500
gooddata:
	.quad	0xDEADC0DE
	.quad	0x15BADC0DE
	.quad	0

# This data is just too long and could cause
# undefined behavior, but isn't (designed to
# be) evil.
baddata:
	.quad	0xCAFEF00D
	.quad	0x155AD
	.quad	0xC0C0A15ACE
	.quad	0xC0FFEE
	.quad	0xD0EABEEF
	.quad	0
	
evildata:
	.quad	0xCAFEF00D
	.quad	0x155AD
	.quad	0xC0C0A15ACE
	.quad	0xC0FFEE
	.quad	exploit
	.quad	0


# This is going to be data that is downright
# malicious and triggers the infinite loop
# problem

# This function is responsible for copying the parameter passed
# to it from main into a local buffer. In real life, it might
# then use that local buffer to do other things; for now, we'll
# just do the copy and return which isn't all that useful.
# input %rdi: the address of a buffer we wish to copy
innocent:
	# This function needs space for a temporary buffer.
	# Such a buffer is a local variable that will be
	# allocated on the stack.

	# PART 1
	# Write the code to create space for a local buffer
	# that is 0x20 bytes large. Place the address of
	# that buffer in %rsi
	
	irmovq 0x20, %rsi
	subq %rsi, %rsp
	rrmovq %rsp, %rsi
	
	# PART 3
	call copy
	
	

innocentDone:
	# PART 2: Add code to teardown the stack frame
	irmovq 0x20, %rsi
	addq %rsi, %rsp

	ret
	
	
	
	.pos 0x2000
# Copies the values in first parameter, 
# into its second parameter.

copy:  #called liked copy(x, y)
	irmovq	8, %r10		# the constant 8
loop:
	mrmovq	0(%rdi), %rax	# %rax = *x
	rmmovq	%rax, 0(%rsi)	# y = %rax
	andq	%rax, %rax	    # set the condition codes
	je	done                # if %rax is == 0, jump to done.

	addq	%r10, %rdi      # x += 8
	addq	%r10, %rsi	    # y += 8
	jmp	loop
done:
	ret
	

.pos 0x3000
exploit:
    nop
    jmp exploit

```