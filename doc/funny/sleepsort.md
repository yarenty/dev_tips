# Sleep SORT algorithm

https://animeshchouhan.com/posts/sleepsort/


## Sorting Algorithms
Sorting algorithms are like the organizers of the computer world, quietly putting everything in order. They help arrange data neatly, whether it's a list of names or scores.

![algo](https://animeshchouhan.com/images/posts/sleepsort/sleepsort.png)


Some sorting methods are simple, like bubble sort, while others are faster, like quick sort. But there are also some really unique ones, like sleep sort, which is more like a fun experiment than a serious tool.

## Sleep Sort
Sleep sort is a fun and quirky way to sort numbers. It was thought up by someone on 4Chan in 2011. Instead of the usual sorting methods, this one makes each number "sleep" for a bit before joining the sorted list. So, if a number is 3, it waits for 3 units of time before settling into its place.

This photo of the original 4chan post was taken from this Princeton Lecture.


![](https://animeshchouhan.com/images/posts/sleepsort/sleepsort-4chan.jpg)



It's not the fastest way to sort numbers, but it's interesting because it's so different. Imagine organizing numbers like you're hitting the snooze button on an alarm clock!

## Dependency on Scheduler
Unlike regular sorting methods that compare and swap numbers, sleep sort does things differently. It relies on the scheduler, which handles tasks running at the same time. Sleep sort works by using this scheduler to time when each number should join the sorted list.


![sched](https://animeshchouhan.com/images/posts/sleepsort/async-eventloop.jpg)


Every modern operating system includes a scheduler, a vital component responsible for managing the execution of various tasks or processes running concurrently. These tasks can range from handling user input to managing system resources efficiently.

This reliance on the scheduler not only distinguishes sleep sort from conventional sorting methods but also underscores the algorithm's dependency on system-level mechanisms for its operation.

## Python Implementation
While we can use the conventional threads to implement this sorting technique, we will be using asyncio python library which is more suited towards blocking IO-bound operations. We would be creating a sleep task for each number in the list and wait till all tasks have finished.

The async function add_num sleeps for the same duration as the number and then appends that to the sorted list.

```python
import asyncio

async def sleepsort(nums):
    sorted_nums = []

    async def add_num(num):
        print(f"Received number {num}, sleeping for {num}s")
        await asyncio.sleep(num)
        print(f"Adding {num} to the list")
        sorted_nums.append(num)

    awaitables = [add_num(num) for num in nums]
    await asyncio.gather(*awaitables)
    return sorted_nums

async def main():
    print(await sleepsort([3, 2, 1, 0, 5, 4]))

asyncio.run(main())

```


Here's a breakdown of how it works:

The sleepsort function takes a list of numbers as input.
Within sleepsort, an empty list called sorted_nums is initialized to hold the sorted numbers.
The inner function add_num is defined to asynchronously add each number to the sorted_nums list after a delay based on the value of the number.
The awaitables list is created by asynchronously calling add_num for each number in the input list.
asyncio.gather is used to concurrently await the completion of all the asynchronous tasks in awaitables.
Once all numbers have been added to sorted_nums, it is returned.

This implementation utilizes the asynchronous nature of asyncio to simulate the sleep sort algorithm, where each number "sleeps" for a duration proportional to its value before being added to the sorted list.

Here's the output when we run this Python script:



![pyth](https://animeshchouhan.com/images/posts/sleepsort/sleepsort.gif)


## Can we make it any faster?
The sole drawback of this algorithm lies in the time consumed during the sleep intervals, which is capped by the highest number within the unsorted input. How can we reduce this sleep delay?

We can try scaling the time spent sleeping to reduce the delay while keeping the order of insertion consistent. Let's give it a shot!

```python
import time
import asyncio

scale = 1e3

async def sleepsort(nums):
    nums_delay = [(num, num / scale) for num in nums]
    sorted_nums = []

    async def add_num(num, delay):
        print(f"Received number {num}, sleeping for {delay}s")
        await asyncio.sleep(delay)
        print(f"Adding {num} to the list")
        sorted_nums.append(num)

    awaitables = [add_num(num, delay) for num, delay in nums_delay]
    await asyncio.gather(*awaitables)
    return sorted_nums

test_nums = list(range(1000, 0, -1))

async def main():
    start = time.perf_counter()
    out = await sleepsort(test_nums)
    time_taken = time.perf_counter() - start
    print(f"Time taken by sleepsort: {time_taken}")

    start = time.perf_counter()
    assert sorted(test_nums) == out
    time_taken = time.perf_counter() - start
    print(f"Time taken by inbuilt sort: {time_taken}")

asyncio.run(main())

```

We opt to divide the numbers by a factor of 1000 to determine the delay in milliseconds. This action decreases our execution time significantly, by a factor of 1000. Previously, the process would have lasted 1000 seconds, but now it only requires 1 second.

This scale can be chosen according to the precision of the system clock for predictability. Let's look at the time taken compared to Python's inbuilt sort function for a reverse sorted list of 1000 numbers:

Time taken by sleepsort: 1.004s
Time taken by inbuilt sort: 0.000057s
The time taken by sleep sort is notably longer at 1.004 seconds compared to Python's built-in sort function, which achieves the same task in an impressively brief 0.000057 seconds.

## Limitations
Sleep sort has its downsides, despite being interesting. Its unpredictability is a big issue because it relies on the scheduler and system load, which can vary a lot. This makes it hard to say how long it will take to sort things. So, it's not great for sorting stuff where you need to know how long it'll take.

It's a neat idea to learn about, but it's not very useful in real life. It's just too unpredictable and not practical for sorting things in the real world. Even though it's fun to play around with, it's not something you'd rely on for serious tasks.

## Conclusion
In summary, Sleep Sort is closely linked to the operating system more than any other sorting method. It shows how the system handles multiple tasks at once. The idea of sorting while sleeping is quite unique.

Even though sleep sort isn't great for big sets of numbers, it shows how creative people can be with sorting algorithms. Overall, it's a fun and quirky algorithm. And as they say, "If it gets the job done, it's not lazy!"

## References
- [Sleep Sort | Hacker News](https://news.ycombinator.com/item?id=2657277)
- [Sleep Sort in JavaScript](https://gist.github.com/Thatkookooguy/3b7ce09a9f80a5e6541175104a5d49e9)
- [Princeton lectures/52Tries.pdf](https://www.cs.princeton.edu/courses/archive/fall13/cos226/lectures/52Tries.pdf)
- [asyncio — Asynchronous I/O — Python 3.12.3 documentation](https://docs.python.org/3/library/asyncio.html)
- [What are the advantages of asyncio over threads?](https://discuss.python.org/t/what-are-the-advantages-of-asyncio-over-threads/2112)


