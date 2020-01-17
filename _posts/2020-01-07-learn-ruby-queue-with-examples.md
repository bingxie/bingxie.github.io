---
layout: post
title: Learn Ruby Queue with Examples
categories: [Tech]
tags: [ruby, queue, rate limiter]
---
In Ruby Language, if we want to use a **FIFO(First In First Out)** Queue data structure, we can use `Array` as a `Queue`. If you want to use a queue object in a `multi-threads` program, then you can choose to use ruby `Queue class`.

Let's learn to use them with some examples:

### Use Array as a Queue
Ruby Array provides us plenty of useful methods.

If we see the last index as the head of the `Queue`, we can use `#unshift` to enqueue, `#pop` to dequeue, and to peek the head element, with this approach we can use `#last` method.

```ruby
queue = Array.new # or []
queue.unshift "dog"
queue.unshift "cat"
queue.unshift "cow"

# queue is: ["cow", "cat", "dog"]

queue.last  # peek returns "dog"

queue.pop # "dog"
queue.pop # "cat"

# queue is: ["cow"]
```

We could see the index 0 as the head of the queue. Then we will use `#push` as enqueue method, `#shift` as dequeue method, and `#first` as the peek method.

```ruby
queue = Array.new # or []
queue.push "dog" # or <<
queue.push "cat"
queue.push "cow"

# queue is: ["dog", "cat", "cow"]

queue.first  # peek returns "dog"

queue.shift # "dog"
queue.shift # "cat"

# queue is: ["cow"]
```
There are another two methods `#size` and `#empty?` used a lot for a queue.
For example when we want to loop through the queue

```ruby
while !queue.empty? do # or queue.size > 0
  item = queue.pop
  # do thing with item
end
```
### Ruby Queue Class
Ruby Queue class is a `thread-safe`, `blocking` queue implementation. And we can use it in a `multi-threaded` program.

We can use `#push`, `#enq` and `#<<` methods to enqueue.

And use `#pop`, `#deq` and `#shift` methods to dequeue.

Those methods are more intuitive than methods in `Array`

The blocking feature is decided by `non_block` argument in `#pop` method, the default value for `non_block` is false, so when the queue is empty, the calling thread is suspended until data is pushed onto the queue.

If `non_block` is true, like `queue.pop(true)`, the thread isn't suspended, and `ThreadError` is raised.

I will create another post to demonstrate how to use `Queue` in a coding interview problem.

### Ruby SizedQueue Class
`SizedQueue` is another Queue implementation in which you can specify the size capacity for the queue. If the capacity is full then the `push` operation will be blocked.

```ruby
sized_queue = SizedQueue.new(3)  # the maximum size is 5

sized_queue.push("a")
sized_queue.push("b")
sized_queue.push("c")

sized_queue.push("d")  # This one is blocked.
```
`#push` method can accept another argument `non_block`, the default value is `false`, if `non_block` is true, the thread isn't suspended, and `ThreadError` is raised.

### Last Thing
There is one disadvantage of using `Queue` and `SizedQueue` classes, they both don't have the `#peek` method to let you peek the head element of the queue.

I will show you how to implement a `thread-safe` `Queue` using `Array` in another Ruby Rate Limiter post.
