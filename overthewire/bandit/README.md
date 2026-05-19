# Bandit

## Introduction

Before the game, please read the [Introduction](https://overthewire.org/wargames/bandit/) and try solving the levels by **yourself** (here is where the real learning happens).

I would also suggest to try to use `man` instead of searching for hint or docs online, 90% of the games commands are fully documented, just use `man`.

If you are stuck, search for hints **not solutions!**
If you can't figure it out after some hints, try doing the level the day after or just read someone's walk through, maybe more than one (I would avoid watching tutorials).

Here you can see my **solutions** and **takeaways**, the `flags` can change but the process should be the same or very similar.

## Lvl 0

**Goal:** Log into the game using SSH. 

If you are not familiar with `ssh`, check the manual: `man ssh`.
If you are not familiar with `man`, check the manual: `man man`.

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

**Takeaway:** `ssh` uses `-p` to specify a non-default port. 

---

## Lvl 0 → 1

**Goal:** Read the `readme` file in the home directory. 

Connect using the credentials from `Lvl 0`.

If you are not familiar with `ls` or `cat` commands, check the manual: `man <command>`.

Use `ls` (optional, we already know the filename) to list files and folders of the current working directory.
Use `cat` to print the `readme` file containing the flag.

```bash
cat readme 
```

**Flag**: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

**Takeaway:** Using `ls` we list the contents of the current directory and using `cat [filename]` we can print the content of a file to the screen. 

---

## Lvl 1 → 2

**Goal:** Read a file named `-` in the home directory. 

Use `./` prefix before `-` to print the file containing the flag.

```bash
cat ./- 
```

**Flag**: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

**Takeaway:** Files named with special characters can be accessed using the `./` prefix.

--- 

## Lvl 2 → 3

**Goal:** Read a file named `--spaces in this filename--` in the home directory. 

Use `./` prefix before `--spaces` to print the file containing the flag.
Use `\` prefix before ` ` spaces.

```bash
cat ./--spaces\ in\ this\ filename-- 
```

Using `""` around the filename is also valid.

```bash
cat "./--spaces in this filename--"
```

**Flag**: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

**Takeaway:** Filenames with spaces can be escaped with `\` before each space ` `, or by wrapping the full path in quotes (`""` or `''`).

--- 
