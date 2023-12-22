# Multithreading Queue in Python and Dining Philosophers Problem
## Multithreading Queue
> In Python, the queue module provides a thread-safe FIFO (First-In-First-Out) data structure that can be used for communication between multiple threads. It helps in managing the flow of data between threads in a synchronized manner, preventing race conditions and data corruption.

>Usage of queue.Queue()
To use a queue in Python, you can create an instance of queue.Queue() and utilize its methods like put() to add items to the queue and get() to retrieve items from the queue.

```python
import queue
import threading

# Create a queue instance
q = queue.Queue()

# Add items to the queue
q.put(1)
q.put(2)
q.put(3)

# Retrieve items from the queue
print(q.get())  # Output: 1
print(q.get())  # Output: 2
print(q.get())  # Output: 3
```
![image](https://github.com/Musa-Sina-Ertugrul/Solution_Dining_P/assets/102359522/df48d7cd-f71e-4037-9ee2-6460edd6ebcd)

## Dining Philosophers Problem
> The Dining Philosophers problem is a classic synchronization problem where a number of philosophers sit at a table and alternate between thinking and eating. There are certain rules to avoid deadlock and ensure that each philosopher can eat without causing a conflict with their neighbors.

![image](https://github.com/Musa-Sina-Ertugrul/Solution_Dining_P/assets/102359522/b8800459-e358-4e74-b81f-75153db01f57)

## Pesudo Solution using Multithreading Queue
Here's an example pesudo implementation using Python's threading and queue modules:

```python
import threading
import time
import queue

class Philosopher(threading.Thread):
    def __init__(self, name, left_fork, right_fork, queue):
        super().__init__()
        self.name = name
        self.left_fork = left_fork
        self.right_fork = right_fork
        self.queue = queue

    def run(self):
        while True:
            self.think()
            self.queue.put((self.name, 'hungry'))
            self.eat()

    def think(self):
        print(f"{self.name} is thinking.")
        time.sleep(1)

    def eat(self):
        self.queue.put((self.name, 'waiting'))
        self.queue.put((self.name, 'waiting for fork', self.left_fork))
        self.queue.put((self.name, 'waiting for fork', self.right_fork))

        while True:
            if self.queue.get() == (self.name, 'forks available'):
                break

        print(f"{self.name} is eating.")
        time.sleep(1)

        self.queue.put((self.name, 'done eating', self.left_fork))
        self.queue.put((self.name, 'done eating', self.right_fork))

        print(f"{self.name} finished eating.")
        time.sleep(1)
        self.queue.put((self.name, 'thinking'))

def main():
    num_philosophers = 5
    forks = [queue.Queue() for _ in range(num_philosophers)]
    philosophers = []

    for i in range(num_philosophers):
        left_fork = forks[i]
        right_fork = forks[(i + 1) % num_philosophers]
        philosopher = Philosopher(f"Philosopher {i + 1}", left_fork, right_fork, forks[i])
        philosophers.append(philosopher)

    for philosopher in philosophers:
        philosopher.start()

if __name__ == "__main__":
    main()
```
This code demonstrates the implementation of the Dining Philosophers problem using threads (Philosopher class) and a queue to control access to forks. Philosophers alternate between thinking and eating while ensuring they acquire both the left and right forks before eating. The queue is used for synchronization between philosophers to avoid conflicts while accessing the forks.

This approach helps in managing the shared resources (forks) efficiently among the philosophers, preventing deadlocks and ensuring fair access to the resources.

This example showcases how Python's multithreading queue (queue.Queue()) can be employed to solve synchronization problems like the Dining Philosophers problem in a thread-safe manner.
