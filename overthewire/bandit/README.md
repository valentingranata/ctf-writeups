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

Use `./` prefix before `--spaces` and `\` prefix before ` ` (spaces) to print the file containing the flag.

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

## Lvl 3 → 4

**Goal:** Find and read the content of a **hidden file** inside the `inhere` directory.

Let's start by changing directory to `inhere`.

```bash
cd ./inhere
```

To see the hidden files inside the current directory, just use the `ls` command with `-a` option.

```bash
ls -a
```

Here we'll see a file named `...Hiding-From-You`, let's print it to the screen.

```bash
cat ./...Hiding-From-You
```

**Flag:** `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

**Takeaway:** Files prefixed with `.` are hidden in Linux. Use `ls -a` to list all files including hidden ones.

---

## Lvl 4 → 5

**Goal:** Find and read the only **human-readable** file inside the `inhere` directory. 

Let's start by changing directory to `inhere`.

```bash
cd ./inhere
```

By doing `ls` inside the `inhere` directory we can see that we have 10 files, let's avoid checking them one by using the `file` command and `*` wildcard (represents "zero or more characters").

```bash
file ./-file*
```

In the output we can see that one of the files is an `ASCII text` file, let's print the content of the file to the screen.

```bash
cat ./-file07
```

**Flag:** `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

**Takeaway:** We can determine the **file type** of a file with the `file` command. Using the `*` wildcard we can match multiple files with one command avoiding manual repetition.

---

## Lvl 5 → 6

**Goal:** We have to find a file with this properties:
- human-readable
- 1033 bytes in size
- not executable 
inside the `inhere` directory.

Let's start by changing directory to `inhere`.

```bash
cd ./inhere
```

By doing `ls` inside the `inhere` folder we'll instantly understand that checking each folder one by one is not a good idea.
Let's use the `find` command to try to filter the files with certain properties.
To get a better understanding of each option used in the command below, check the manual for `find` command: `man find`.

```bash
find ./ -type f -size 1033c -not -executable
```

After the `find` command we'll see only one file seems to match our search, let's print the content of that file to the screen

```bash
cat ./maybehere07/.file2
```

**Flag:** `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

**Takeaway:** We can use the `find` command to filter files by type (`-type f`), size (`-size 1033c`, where `c` = bytes), and permissions (`-not -executable`), making it powerful for locating specific files in large directory trees.

---

## Lvl 6 → 7

**Goal:** Find a file with the following properties: 
- owned by user `bandit7`
- owned by group `bandit6`
- `33 bytes` in size

When we login we can see that there is nothing inside the home directory, not even `ls -a` can save us.

Let's try to use the `find` command on the root `/` directory with the required properties.

```bash
find / -user "bandit7" -group "bandit6" -size 33c 2> /dev/null
```

To avoid useless `Permission denied` messages, we can redirect the standard error (`stderr`) to `/dev/null` folder that acts as a "black hole" on linux, deleting anything that's going inside.

Now we should see only one file, let's print it to the screen.

```bash
cat /var/lib/dpkg/info/bandit7.password
```

**Flag:** `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

**Takeaway:** We used the `find` command again but with different options, such as: `-user` and `-group` (to specify the owner and group of the file). We also learned how to redirect `stderr` to `/dev/null` (`... 2> /dev/null`), a special file that acts as a **black hole** (anything written to it is permanently discarded).

---

