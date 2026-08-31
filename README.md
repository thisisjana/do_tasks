# do_tasks
# To-Do Manager

A minimal, clean To-Do List Management System, written in C, running entirely in the terminal.

## Overview

To-Do Manager- DO TASKS lets a user add, update, delete, and view tasks from the command line. It's a fixed-scope implementation — no logins, no databases, no gamification — built to demonstrate that a simple problem can still be solved with a thoughtful, uncluttered user experience. The interface relies on generous spacing and consistent alignment rather than borders or decoration.

## Problem Statement

This project satisfies a club screening task: implement a To-Do List Management System providing Insert, Delete, Update, Display, and Exit operations, with a unique ID per task, and correct handling of empty lists, invalid IDs, and invalid menu input.

## Features

- **Insert Task** — add a new task with a description; receives a unique Task ID
- **Delete Task** — remove a task by ID, with a Y/N confirmation before deleting
- **Update Task** — replace the description of an existing task by ID
- **Display Tasks** — view all tasks in an aligned table
- **Exit** — leave the program from the main menu
- Handles an empty task list, invalid/non-existent Task IDs, a full task list, empty task descriptions, and invalid menu choices — every case shows a clear message, none of them crash the program
- The menu keeps redisplaying until Exit is chosen

No functionality beyond this list is implemented.

## Design

Every screen shares the same layout language: a consistent left margin, a single plain divider line (`-`) where one is needed, and generous blank lines between sections instead of borders or boxes. There's no color and no special symbols — just spacing and alignment doing the visual work. This keeps the program fully portable (no ANSI codes, no Unicode reliance) and keeps the formatting code itself small, since the whole interface is built from one small divider helper plus plain `printf` calls.

## Technologies

- C (C99)
- Standard library only: `stdio.h`, `string.h`
- No external libraries, no dynamic memory allocation, no files/databases, no ANSI color codes

## Requirements

Any standard C compiler, e.g. GCC. Tested with `gcc` on Linux; works the same way on macOS and Windows (via MinGW/GCC) since nothing platform-specific is used.

## How to Compile

```bash
gcc source_code.c -o todo
```

## How to Run

```bash
./todo
```

On Windows (MinGW/GCC), the same compile command produces `todo.exe`; run it with `todo` or `.\todo.exe` from the terminal.

## Menu

```
1   Add a task
2   Delete a task
3   Update a task
4   View all tasks
5   Exit
```

## Example Usage

```
        To-Do Manager

        Simple tasks. Clear mind.

        ----------------------------

        1   Add a task
        2   Delete a task
        3   Update a task
        4   View all tasks
        5   Exit

        ----------------------------

        >  1

        Add a Task

        Enter task description:
        > Finish C assignment

        Task added successfully.
        Task ID: 01
```

## Error Handling

- **Invalid or non-existent Task ID** (delete/update) — a plain two-line message, the task list is left unchanged
- **Empty task list** — a dedicated "no tasks yet" message appears instead of an empty table
- **Empty task description** — rejected with a message; no task is added/updated
- **Invalid menu choice** (non-numeric or out of range) — a one-line message, then the menu is shown again
- **Full task list** — insertion is rejected with a clear message once the maximum of 50 tasks is reached
- **Long descriptions** — truncated with "..." in the task table so a single task can never break the layout; input beyond the buffer size is also safely discarded from the input stream so it can't corrupt the next read (see `readLine()` in the source)

## Task ID Strategy

Task IDs come from a single counter that starts at 1 and only ever increases — it is never recalculated from the array or reused. Deleting a task shifts later tasks left in the array to close the gap, but IDs are independent of array position, so a newly inserted task always gets a genuinely new ID, even after earlier tasks have been deleted.

## Project Structure

```
do_tasks/
├── source_code.c
└── README.md
```

## Future Improvements

- Persist tasks to a file so they survive between runs
- Allow marking a task as done without deleting it
- Support editing a task's description inline without retyping the whole thing

---

*Built as a submission for a club's C programming screening task.*
