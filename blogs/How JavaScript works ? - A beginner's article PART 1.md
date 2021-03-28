# How JavaScript works ? - A beginner's article { PART 1 }
sources:
- https://www.youtube.com/watch?v=hGSHfObcVf4
- [diagrams]()
- https://blog.bitsrc.io/how-does-javascript-work-part-2-40cc15360bc
- [absolute Guide](https://dev.to/lydiahallie/javascript-visualized-the-javascript-engine-4cdf)
- [Colt Steele's Call back youtube video](https://www.youtube.com/watch?v=W8AeMrVtFLY)
> Things we will talking in this article :
	> 1. Javascript Engine
		> 1.1 Memory Heap
			> 1.1.1 Event listeners
			> 1.1.2 Removed DOM element


As a web developer without any degree sometimes i feel like i am missing some foundations in computer science and unfortunately it is true. The only thing I know for sure is that I am a human being and I can think and understand. 

So I thought why i couldn't start from the absolute basics of web development. Thus I started learning about Javascript.

These are the questions I asked to myself and answers I found when I learned.
## What is a programs ?
A program has to do two things basically :
1. Allocate memory : to store the variables, constants, functions etc during the execution of the program.
2. Parse and execute : Simple means read and run commands in the program.

In case of javascript we use any engine that comes inside the browser to parse and execute the code. eg : V8 engine present in chrome browser. This engine converts the code you write in JS to machine understandable instructions for the browser.

## Javascript Engine

Javascript Engine consists of two parts:
1. Memory heap
2. Call stack

### 1. Memory Heap
Memory heap is where the memory allocation happens.

![[Pasted image 20210328215923.png]]

When we declare a constant like
```

const a = 10;

```
the javascript engine will allocate a specific allocation to store the value. Intially it will be having a data-type `undefined`.
You might have heard the word **memory leak**  a lot (i guess !). This happens if our javascript engine allocates a memory for anything and we are not using that constant/variables/object in our code rather than declaring then that memory allocated by the engine wasted (right !). So whenever we declare unused variables/constants/object that will fill up the memory heap. This is why we generally avoid global declarations, so that we can reduce the allocations of unused memory and their cleaning ups. Javascript engine itself has a garbage collector to reclaim the unused memory of an out of context object/variable and return back that memory to the free memory pool. eg: Garbage collector in chrome's V8 engine is called as *Orinoco*.
#### Example of a common memory issue:
![[Pasted image 20210328214524.png]]
> I have experienced this issue in one of my react projects where i declare all the states in a parent component which has numerous amount of child and grandchildren. I used those states inside these children which will only render on demand.

#### Event Listeners
The event listeners you add to your website components for better experience also leads to memory leaks. When user navigate between the same pages these events keeps adding up.

#### Removed DOM elements:
Even if you destroy a DOM element ( generated in global scope ) by a handler, the reference of the element will be still existing and so does its allocated memory. Thus make sure we generate most of the DOM elements inside a local scope, so as the scope terminates ,its allocated memory gets reclaimed by the garbage collector.

#### Memory Life cycle:

![[Pasted image 20210329000525.png]]
 
### 2. Call stack
In call stack the engine runs and executes the code.
Stack in programming just means **LAST IN**  -  **FIRST OUT**. We can easily understand using an example.

```
/*
* In this code i need to get ready for school -> go to bus stop -> find a bus -> find a seat
/
const findSeat() {
	return isSealAvailable
}

const findBus() {
	isBusExist && findSeat()
}

const goToBusStop() {
	amIReady && findBus()
}

goToBusStop()

```

![[Pasted image 20210329001328.png]]
- Whenever we call a function, the details of the invocation are saved to the top of stack (here we can say it as stack bucket )
- As soon as the function returns something it will be cleared up from the stack
- That is, whatever comes at the top of the stack gets completed and removed from the stack as the first one.
- #### What is stack frame.
	- A single frame you see inside the stack *bucket* is called as stack frame.

	- ![[Pasted image 20210329001946.png]]

	- The contents of a single stack frame will be
		- function	invoked.
		- Parameters passed to the function.
		- Current line number

#### How call stack will work for this code ?


## Why JavaScript is single threaded ?
A single threaded language is simple in execution which eliminates the complexities a multi threaded language will be going through. for eg: DeadLocks.

