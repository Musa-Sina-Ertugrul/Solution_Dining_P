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
> The Dining Philosophers problem is a classic synchronization problem in computer science that illustrates issues with resource allocation and concurrent processes. It was introduced by E.W. Dijkstra in 1965 to demonstrate the challenges of avoiding deadlock and ensuring the safe sharing of limited resources among multiple processes.

>The problem is framed around a scenario where a certain number of philosophers are seated at a round dining table, with a bowl of spaghetti placed between each pair of adjacent philosophers. The philosophers spend their time alternating between two activities: thinking and eating.

>Each philosopher has a few basic actions:

>Thinking: Philosophers spend their time thinking until they become hungry.

>Eating: To eat, a philosopher needs two forks - one for the left hand and one for the right hand. They pick up the forks, eat for a while, and then put the forks back on the table.

>The challenge arises from the following conditions:

>Each philosopher needs two forks to eat.
Philosophers only take one fork at a time.
Philosophers do not communicate with each other directly, only through the shared forks.
The goal is to design a solution (algorithm or set of rules) that allows the philosophers to dine without deadlocking, where deadlock occurs when each philosopher holds one fork and waits indefinitely for the other, resulting in all philosophers being stuck and unable to continue.

>Solving the Dining Philosophers problem involves creating a set of rules or protocols to prevent deadlocks and ensure fair access to resources (forks in this case). Some common strategies to address this problem include:

> * Resource Allocation: Implement a mechanism that allows philosophers to access forks without causing deadlock. This could involve assigning a priority order for philosophers to pick up the forks, ensuring that no philosopher can starve.

> * Asymmetric Solution: Introduce an asymmetry where one philosopher picks up forks in a different order compared to others, breaking the possibility of a circular wait condition.

> * Mutex (Mutual Exclusion): Use locks or semaphores to ensure exclusive access to shared resources (forks) and prevent multiple philosophers from simultaneously accessing the same fork.

> * Arbitrator: Introduce a central authority or arbitrator that controls the access to forks, ensuring that philosophers can only access forks when available and preventing deadlocks.

> The Dining Philosophers problem serves as an example in concurrent programming and illustrates the challenges of resource sharing, synchronization, and avoiding deadlocks in systems with multiple competing processes. Solutions to this problem often demonstrate synchronization techniques and strategies for managing shared resources among concurrent processes efficiently and safely.

![image](https://github.com/Musa-Sina-Ertugrul/Solution_Dining_P/assets/102359522/b8800459-e358-4e74-b81f-75153db01f57)

## Pesudo Solution using Multithreading Queue
> Here's an example pesudo implementation using Python's threading and queue modules:

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
> This code demonstrates the implementation of the Dining Philosophers problem using threads (Philosopher class) and a queue to control access to forks. Philosophers alternate between thinking and eating while ensuring they acquire both the left and right forks before eating. The queue is used for synchronization between philosophers to avoid conflicts while accessing the forks.

> This approach helps in managing the shared resources (forks) efficiently among the philosophers, preventing deadlocks and ensuring fair access to the resources.

> This example showcases how Python's multithreading queue (queue.Queue()) can be employed to solve synchronization problems like the Dining Philosophers problem in a thread-safe manner.

![image](https://github.com/Musa-Sina-Ertugrul/Solution_Dining_P/assets/102359522/e8a78821-bd21-4bc5-838f-706114d9ccec)

