# Exam answers

## Question 1

### 1. A

*Explain the difference between logical and arithmetic shifting*.

#### 1.A Written solution

A *Logical* shift is a bitwise operation that shifts all the bits of its operand.
These can be useful to perform multiplication or division in an efficient way.

Bit-shifting is just multiplying and dividing by powers of two to add or remove digits.

With *Logical* shifts, a number's sign bit is not preserved. And, a number's exponent is not distinguished from its significand. Instead, every bit is simply moved a given number of bit positions. All empty bit-positions will be filled with zeros.

An *Arithmetic* shift is like a *Logical* shift, except for the important difference that vacant bit-positions are not simply filled with all zeroes when shifting to the right. Instead, the leftmost bit - which will usually by the sign bit for signed integers - will be replicated to fill in all the vacant positions.

#### 1.A Exam version

- Forklar forskellene bedre - sæt det i forhold til Two's-Complement hvor et logical shift ville sætte 0er ind foran og få det til at se ud som om at tallet er positivt - uanset om det er det eller ej - i modsætning til aritmetisk, som beholder sign bit.

### 1. B

*Write a C program that takes a long and computes its absolute value. Describe the steps you are taking to compute the absolute value*.

#### 1. B Written solution

```c
int main(int argc, char **argv) {
	// The first argument will be the integer to compute the absolute value of
	int raw = argv[1];

	// The target for the absolute value
	unsigned int target;

	// Create a mask that is equal to all 1s if the sign bit is set.
	int const mask = raw >> sizeof(int) * CHAR_BIT - 1;

	// Compute the absolute value
	target = (raw + mask) ^ mask;

	printf("%d", target);

	return 0;
}
int v;           // we want to find the absolute value of v
unsigned int r;  // the result goes here 
int const mask = v >> sizeof(int) * CHAR_BIT - 1;

r = (v + mask) ^ mask;
```

#### 1. B Exam version

Forklar hvad en bitmask er, forklar hvad en absolut værdi er i denne sammenhæng.

## Question 2

### Question 2. A

*What is a stack canary?*.

#### 2. A Written solution

A *Stack canary* is a technique for preventing malicious attempts at causing an overflow in the heap and overriding memory spots that the program shouldn't be able to access.

This can happen since any program that prompts for user input has an input buffer that it stores the input in. But this buffer is at all times of a specific length. Not only can it be filled, it can also be overflowed.

With the Stack canary technique, a random value, the *canary*, is placed between the local variables and the return address of a function call. It builds on the principle that local variables are stored on the call stack, which stores the return address of the function.

Before the functon returns, the program checks that the canary hasn't been altered.

An attacker can still get around a stack canary by gaining control of a pointer that the attacker can then use to write the return address directly.

#### 2. A Exam version

Tegn det op, vis hvordan det ville se ud i maskinkode.

### Question 2. B

*Can they be used as a counter-measure for the exploits you designed in Assignment 2? Explain your answer.*.

#### 2. B Written solution

Yes, they can. In Assignment 2, we didn't use any Stack canaries and could easily exploit it.

We could have checked the canaries for manipulation before returning from the function calls to ensure that the return address(es) hadn't changed, which would have avoided any of the exploits we made.

#### 2. B Exam version

Forklar i flere detaljer. Check koden igennem. Giv et helt konkret eksempel.

## Question 3

*Consider the logfind tool from Assignment 3*.

### Question 3. A

*Desribe the glibc functionalities you use in your program*.

#### 3. A Written solution

- `Malloc`: Allocates the given size of bytes of memory and returns a pointer to the allocated memory location.

- `strlen`: Computes the length of the given string. It attempts to compute the length, but never scans beyond the first `maxlen` bytes of s.

- `strncmp`: Performs a lexicographical comparison between two null-terminated strings.

- `glob`: Searches for all the pathnames matching the given pattern according to the rules used by the shell.

- `fopen`: Opens the file whose matching the given location and associates a stream with it.

- `fclose`: Disassociates the named stream from its underlying file.

#### 3. A Exam version

Gå i dybden med forskellen mellem `strcmp` og `strncomp`. Hvorfor du kan risikere buffer overflow hvis du ikke eksplicit giver en længdebegrænsning (n).

Forklar om malloc og hvad du brugte den til. Gå opgaven igennem.

### Question 3. B

*Using grpof to profile your program, describe how your program performs when the size of the input sequence is varied from 1 word to 10 words. Discuss how time is spent during the execution of your program. Does performance depend on the order of the words?*.

#### 3. B Written solution

Due to short circuiting conditions, the order in which the interpreter stumples upon conditions in boolean AND/OR operations matter. With OR, the operation terminates as soon as a condition is met that is truthy. With AND, all of the conditions must be checked for truthiness.

Therefore it makes sense to sort the conditions differently based on the kind of operation to perform. For AND operations, we want it to terminate as quickly as possible. So if we provide the most uncommon words first, there is a greater chance that a falsy condition is met as one of the first ones and that the operation will terminate.

For OR operations, it is the other way around - we want to have the most common words in front since the OR expression will terminate as soon as it stumples upon a truthy one.

#### 3. B Exam version

Her skal du nok konkretisere dit svar meget og gå opgaven igennem. Der tænkes måske også på asymptotisk kompleksitet her. Hele det der med at optimisere koden, e.g. tage ting ud af loops så de ikke skal evalueres unødvendigt mange gange.

## Question 4

### Question 4. A

*What is an implicit free list?*.

#### 4. A Written solution

When performing manual memory allocation, as we do in C, the *free list* is a convenient data structure for building dynamic memory allocation. where unallocated regions of memory is connected in a *linked list*.

With *implicit* free lists, we store the length and whether or not the block is allocated for each block. That can be pretty wasteful if store the information in two words, so instead we take advantage of the fact that some low-order address bits are always 0 if the blocks are aligned. And, instead of storing the 0-bit for all of them, we use that one as a flag to determine if the block is allocated or free.

Implicit free lists are easy to implement, but may take O(n) for allocation which makes it inefficient for usage with `malloc()` or `free()` operations.

#### 4. A Exam version

Illustrér på tavlen. Sæt det i forhold til explicit free list og segregated free list. Forklar hvorfor det kan tage O(n) tid (first-fit, next-fit, best-fit, holder reference til alle blocks fremfor kun *free*).

### Question 4. B

*Describe your design and implementation of a heap checker in Assignment 4*.

#### 4. B Written solution

(None)

#### 4. B Exam Version

Check opgaven igennem for at forstå hvad det går ud på, så du har et eller andet at sige her.

## Question 5

### Question 5. A

*What is virtual memory?*.

#### 5. A Written solution

Virtual Memory is a memory management technique that sits on top of physical memory that treats the main memory as a cache. This simplifies memory management greatly by providing each process with a uniform address space and protects the address space from corruption by other processes. This makes processes memory-decoupled from each other and serves as a mapper between physical- and virtual memory locations.

#### 5. A Exam version

Det er et meget tyndt svar.
Snak om MMU og TPL, om DRAM cache og om hits/misses. Snak om hvorfor det er smart - locality.
Snak om at hver process har sin egen page table og sit eget virtuelle address space.

### Question 5. B

*What is a Translation Lookaside Buffer?*.

#### 5. B Written Solution

The Translation Lookaside Buffer (TLB) is a small, virtually addressed cache. Since mapping between virtual and physical memory can be challenging and the lookups are repeated often, it makes sense to cache these mappings so that the CPU can access it directly from within that cache rather than computing the mapping again.

#### 5. B Exam Version

Illustrér på tavlen. Forklar hvorfor den er smart.
Nævn at kan spare nogle CPU cycles osv.

### Question 5. C

*What happens when there is a page fault?*

#### 5. C. Written solution

When a Page Fault happens, a program attempts to access a memory page that the virtual address space of the process doesn't know about. It *may* be accessible by the process, but the memory management doesn't know about it since it isn't mapped to any virtual memory location.

These faults not necessarily errors, but can also trigger the OS to attempt to figure out where that memory lives and will read the page from disk and finally reconfigure the virtual addressing system provided to the process to perform the mapping to the actual location.

### 5. C Exam version

Tegn på tavlen diagrammet over at PTE ikke har en adresse i DRAM og derfor laver en page fault. Forklar så hvordan page fault exception handleren tager den virtuelle page fra disken, finder et "victim" i DRAM cachen og erstatter den med den virtuelle page og dernæst sætter den nye adresse på PTE'en.

### Question 5.D

*Give an example of a C program that generates a segmentation fault*.

#### 5. D Written solution

A segmentation fault (*"segfault"*) happens when a process attempts to access a memory location that doesn't belong to it.

They most often happen because of pointer errors in dynamic memory allocation. But they can also happen if we attempt to write to a read-only memory location as in this example:

```c
int main(void) {
    char *s = "foobarbaz";
    *s = 'bazbarfoo';
}
```

This will produce a segmentation fault on memory protected systems since it attempts to modify a string literal (which is read-only)

#### 5. D Exam Version

Illustrer hvor i virtual memory at statisk deklarered strenge havner henne og hvorfor det springer i luften.