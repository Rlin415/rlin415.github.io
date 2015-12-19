---
layout: post
title: Data Structures
---

Data Structures are constructs that store data for us. However, there can be many ways in
which data can be manipulated and used, which is why there are so many different types of data structures
out there. I'm going to write about two basic, but principal ones here: stacks and queues.

### Stack

A stack is a data structure that can be implemented with an array or hash object. Implementing it with an
array would be at the cost of O(n) time lookup for data in the stack. For that reason, we should always try
to use hash objects when capable which reduces lookup time to O(1). Stack has two main methods: pop and
push. Pop removes the last element in it's storage while push adds an element to the end of it's storage.
This is also where Stack differentiates itself in. It is a last in, first out process.

![alt text](http://upload.wikimedia.org/wikipedia/commons/thumb/2/29/Data_stack.svg/200px-Data_stack.svg.png "Logo Title Text 1")

A good example of when we could use this type of data structure is while implementing a browser's back button. Every time
we go to a new webpage on the same tab, we "push" the previous webpage to a stack so that when users click the back
button we can "pop" it out and switch our browser's view to that previous webpage.

### Queue

A queue is a relative of the stack and is implemented the same way. Accomplishing the queue with an array
would also be at the cost of O(n) time lookup for the data in it, while with a hash object will reduce that time to
O(1). Queue has two main methods just like the stack: enqueue and dequeue. Enqueue adds an element to the front of
the queue's storage, while dequeue removes the first element from the storage. This is where it differentiates itself from
stack. It is a first in, first out process.

![alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Data_Queue.svg/200px-Data_Queue.svg.png "Picture of a stack")

A good example of when you might use a queue is while implementing a media player. When a user clicks on a song in the media player
you would want that media player to "enqueue" that song to the queue so that it becomes the first song in line to play.
We want all previous songs the user clicked on to play AFTER the song that was most recently clicked on by the user.
That's why we want to add things to the front of the data structure instead of the back like in a stack.
After the song in the front of the line is finished playing, we want to "dequeue" it out of the queue so the song after
it plays.

### Conclusion

Each type of data structure has it's own unique characteristic. Each one is meant to do a different type of job.
One may be better for a certain problem, like how a stack is better used for creating a browser's back button;
while another may be better for a different kind of problem, like how a queue is better used for creating a
media player. It is important for a developer to understand the differences between types of data structures
so they can utilize the best one for the job.
