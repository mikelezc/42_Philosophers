# Philosophers

A C implementation of the classic Dining Philosophers Problem.

## Overview

This project is an implementation of the Dining Philosophers Problem, a classic synchronization and concurrency problem in computer science. It simulates a scenario where a group of philosophers sit around a circular table, alternating between thinking, eating, and sleeping. The challenge is to ensure that all philosophers can eat without causing deadlocks or resource starvation.

## Problem Statement

- A certain number of philosophers sit around a circular table
- There is a fork between each pair of philosophers
- Each philosopher needs two forks to eat (one from each side)
- Philosophers can only do three things: eat, sleep, and think
- If a philosopher doesn't eat for a specific amount of time, they will die
- The simulation stops when a philosopher dies or when all philosophers have eaten a certain number of times

## How to Build

Clone the repository and compile the program:

```bash
git clone git@github.com:mikelezc/42_Philosophers.git Philosophers_42
cd Philosophers_42
make
```

The compilation will generate an executable named `philo`.

## Usage

Execute the program with the following parameters:

```bash
./philo number_of_philosophers time_to_die time_to_eat time_to_sleep [number_of_times_each_philosopher_must_eat]
```

Parameters:
- `number_of_philosophers`: Number of philosophers and forks
- `time_to_die`: Time in milliseconds after which a philosopher dies if they haven't started eating
- `time_to_eat`: Time in milliseconds that a philosopher takes to eat
- `time_to_sleep`: Time in milliseconds that a philosopher spends sleeping
- `[number_of_times_each_philosopher_must_eat]`: Optional parameter, simulation stops when all philosophers have eaten at least this many times

## Testing Guidelines

### Constraints

- Do not test with more than 200 philosophers
- Do not test with time values lower than 60 ms for `time_to_die`, `time_to_eat`, or `time_to_sleep`

### Test Cases

Here are some test cases to validate the program:

1. `./philo 1 800 200 200`
   - The philosopher should not eat and should die

2. `./philo 5 800 200 200`
   - No philosopher should die

3. `./philo 5 800 200 200 7`
   - No philosopher should die and the simulation should stop when every philosopher has eaten at least 7 times

4. `./philo 4 410 200 200`
   - No philosopher should die

5. `./philo 4 310 200 100`
   - One philosopher should die

### Tests with 2 philosophers

Test with 2 philosophers and verify timing precision (a death delayed by more than 10 ms is unacceptable):

- `./philo 2 800 200 200` - No philosopher should die
- `./philo 2 401 200 200` - No philosopher should die
- `./philo 2 400 200 200` - One philosopher should die

### Edge Cases

Test with tight timing constraints:

- `./philo 3 310 103 103`
  - 3 philosophers, with 310 ms time to die
  - Eating takes 103 ms, so 103 × 3 = 309 ms
  - Only 1 ms margin before death

- `./philo 2 401 200 200`
  - Similar edge case with 2 philosophers

### Debugging Techniques

- For thread race condition detection:
  ```
  -fsanitize=thread -g3
  ```

- For memory leak checks:
  ```c
  void myleaks(void)
  {
      system("leaks philo");
  }
  
  atexit(myleaks);
  ```

## Project Structure

The project is organized into the following components:

- `args.c` - Command-line argument processing
- `eat.c` - Philosopher eating actions
- `finish.c` - Termination and cleanup
- `p_ther.c` - Monitoring thread functionality
- `philo_actions.c` - Main philosopher behavior
- `philo.c` - Entry point and initialization
- `set_scenario.c` - Setup of environment and resources
- `start_dinner.c` - Start simulation and threads
- `time.c` - Time-related utilities

## Notes

This project is part of the curriculum at [42 School](https://42.fr/).