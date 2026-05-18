# push_swap

A sorting algorithm project from the 42/1337 curriculum. Sorts a stack of integers using only a fixed set of stack operations, outputting the shortest sequence of moves possible.

## How it works

The program receives a list of integers and sorts them using two stacks (`a` and `b`) and the following operations:

| Operation | Description |
|-----------|-------------|
| `sa` / `sb` | Swap the top two elements of stack a / b |
| `ss` | `sa` and `sb` simultaneously |
| `pa` / `pb` | Push the top of b onto a / top of a onto b |
| `ra` / `rb` | Rotate stack a / b upward (top becomes bottom) |
| `rr` | `ra` and `rb` simultaneously |
| `rra` / `rrb` | Reverse rotate stack a / b (bottom becomes top) |
| `rrr` | `rra` and `rrb` simultaneously |

### Algorithm

Uses a **chunk-based approach**:

1. Assigns each element a sorted index (1 to n) to avoid working with raw values.
2. **Centralize** — splits stack A into chunks by index range and pushes them to stack B in batches. Elements in the lower half of a chunk are rotated to the bottom of B to keep things roughly ordered.
3. **sort_three** — sorts the remaining 3 elements in A with at most 2 operations.
4. **sort_all** — merges stack B back into A by repeatedly finding and pulling the next expected element, using rotations to minimize moves.
5. A final reverse-rotate pass ensures A starts at index 1 (the minimum).

## Build

```bash
# Build push_swap
make

# Build the bonus checker
make bonus

# Clean object files
make clean

# Full clean (removes executables too)
make fclean

# Rebuild
make re
```

## Usage

```bash
# Sort numbers
./push_swap 3 1 4 1 5 9 2 6

# Pipe into checker (bonus) to verify the result
./push_swap 3 1 4 1 5 9 2 6 | ./checker 3 1 4 1 5 9 2 6

# Count the number of operations
./push_swap 3 1 4 1 5 9 2 6 | wc -l
```

The checker reads the same arguments and the operation list from stdin, then prints `OK` if the stack ends up sorted or `KO` otherwise.

## Project structure

```
push_swap/
├── pushswap/
│   ├── push_swap.c        # Entry point
│   ├── include/push_swap.h
│   └── src/
│       ├── sorting.c      # Core algorithm (centralize, sort_three, sort_all)
│       ├── instructions.c # Stack operations (sa, pb, ra, rrb, ...)
│       ├── parsing.c      # Argument validation and stack initialization
│       ├── helpers.c      # Linked list utilities
│       └── libft.c        # Wrappers around libft used by push_swap
├── bonus/
│   └── checker_bonus.c    # Interactive checker program
├── libft/                 # Custom C standard library
└── Makefile
```
