# Philosophers 🍝

This project is a solution to the classic **Dining Philosophers problem**, implemented in C using **threads** and **mutexes**. It is part of the 42 curriculum (Rank 3).

## 📝 Description

The simulation mimics the life of philosophers sitting around a round table.
- They have 3 states: **Eating**, **Sleeping**, **Thinking**.
- There is one fork between each pair of philosophers.
- A philosopher needs **two forks** to eat (left and right).
- They must not starve (time_to_die).
- The simulation stops if a philosopher dies or if they all have eaten enough (optional).

## 🛠️ Usage

### Compilation
Use the provided Makefile to compile the project:
```bash
make
```

### Execution
Run the program with the following arguments:
```bash
./philo [number_of_philosophers] [time_to_die] [time_to_eat] [time_to_sleep] [number_of_times_each_philosopher_must_eat]
```

- **number_of_philosophers**: The number of philosophers (and forks).
- **time_to_die** (ms): If a philosopher doesn't start eating 'time_to_die' ms after their last meal, they die.
- **time_to_eat** (ms): Time it takes to eat (holding two forks).
- **time_to_sleep** (ms): Time spent sleeping.
- **[number_of_times_each_philosopher_must_eat]** (optional): Simulation stops if all philosophers have eaten at least this many times. If not specified, simulation stops only at death.

### Example
```bash
./philo 4 410 200 200
```
*4 philosophers. They die if they don't eat for 410ms. Eating takes 200ms, sleeping 200ms.*

## 💡 Implementation Details

The project relies on specific system concepts to ensure concurrency and avoid data races:
- **Threads (`pthread`)**: Each philosopher is a thread.
- **Mutexes (`pthread_mutex`)**: Used to protect shared resources, specifically:
  - **Forks**: Each fork is a mutex to prevent two philosophers from holding it simultaneously.
  - **Printing**: To prevent scrambled output messages.
  - **Data Access**: To prevent data races when reading/writing shared variables (like `last_meal` or `dead` status).

## ⚠️ Key Features

- **Deadlock Prevention**: Even-numbered philosophers wait a small delay at startup to allow odd-numbered philosophers to grab forks first, establishing a rhythm and preventing circular wait.
- **Monitoring**: A separate monitoring thread constantly checks for starving philosophers to ensure death is detected promptly.