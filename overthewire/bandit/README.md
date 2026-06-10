# Bandit

## Introduction

Before the game, please read the [Introduction](https://overthewire.org/wargames/bandit/) and try solving the levels by **yourself** (here is where the real learning happens).

I would also suggest to try to use `man` instead of searching for hint or docs online, 90% of the games commands are fully documented, just use `man`.

If you are stuck, search for hints, **not solutions!**
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

Use the `./` prefix before `-` to print the file containing the flag.

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

To see the hidden files inside the current directory, just use the `ls` command with the `-a` option.

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

By doing `ls` inside the `inhere` directory we can see that we have 10 files, let's avoid checking them one by one, let's try to use the `file` command and `*` wildcard (represents "zero or more characters").

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

**Goal:** We have to find a file with these properties:
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

After the `find` command we'll see that only one file seems to match our search, let's print the content of that file to the screen

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

To avoid useless `Permission denied` messages, we can redirect the standard error (`stderr`) to `/dev/null`, a special file that acts as a "black hole" on Linux, deleting anything that goes into it.

Now we should see only one file, let's print it to the screen.

```bash
cat /var/lib/dpkg/info/bandit7.password
```

**Flag:** `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

**Takeaway:** We used the `find` command again but with different options, such as: `-user` and `-group` (to specify the owner and group of the file). We also learned how to redirect `stderr` to `/dev/null` (`... 2> /dev/null`), a special file that acts as a **black hole** (anything that goes into it is permanently discarded).

---

## Lvl 7 → 8

**Goal:** Search for the flag inside `data.txt` file next to the word `millionth`. 

To find the flag we can use the `grep` command with the following pattern `"millionth"` on the `data.txt` file.

```bash
grep "millionth" data.txt
```

**Flag:** `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc` 

**Takeaway:** Instead of reading a huge file line by line, we can use the `grep` command to print only the line that matches a **pattern**. 

---

## Lvl 8 → 9

**Goal:** Find the flag inside `data.txt`, it's the only line of text that occurs only once. 

To find the flag inside the `data.txt`, we first need to **print** the data, **sort** them and use the `uniq -c` command to print the only unique line in the sorted file (it's important for the `uniq` command to work properly that the data is sorted).

```bash
cat data.txt | sort | uniq -u
```

**Flag:** `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM` 

**Takeaway:** The `|` (pipe) operator passes the output of left command to the input of the right command. `sort` orders lines alphabetically, and `uniq -u` prints only lines that appear exactly once (but `uniq` requires sorted input to work correctly).

---

## Lvl 9 → 10

**Goal:** Find the flag in the `data.txt` file, the flag is preceded by several `=` chars. 

To solve this level we have to use the `strings` command to print all printable characters in the file, pipe `|` the result with `grep` to filter for `=` chars. 

```bash
strings data.txt | grep "====="
```

**Flag:** `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey` 

**Takeaway:** The `strings` command extracts **human-readable text** from binary files. Combined with `grep`, it lets us filter for specific patterns (here the `=` characters that precede the flag).

---

## Lvl 10 → 11

**Goal:** Read the flag from `data.txt` file encoded in base64.

We can use the `base64` command with the `-d` option to **decode** a base64 encoding.

```bash
base64 -d data.txt
```

**Flag:** `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr` 

**Takeaway:** When working with base64 encoding we can use the `base64` command to **encode** or **decode** the data. 

---

## Lvl 11 → 12

**Goal:** Rotate the text inside the `data.txt` file by 13 positions. 

To rotate the letters by 13 positions, we can print the `data.txt` file and pipe it into the `tr` command to map each letter to the one 13 positions forward in the alphabet.

```bash
cat data.txt | tr [a-zA-Z] [n-za-mN-ZA-M]
```

**Flag:** `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4` 

**Takeaway:** We can replace or delete chars by using the `tr` command. 

---

## Lvl 12 → 13

**Goal:** Decompress `data.txt` multiple times to find the **flag**. 

First as suggested, let's create a temporary folder to work in.

```bash
mktemp -d
cd <path/to/temp/folder>
```

Let's copy the `data.txt` file to the temporary folder.

```bash
cp ~/data.txt ./
```

If we print the `data.txt` file, we can see that it's a hexdump of a binary, so let's convert it back to a binary with the `xxd -r` command.

```bash
xxd -r data.txt > binary.gz
```

Now we'll check the file type with the `file` command and decompress the data until we find the **flag**.

```bash
gzip -dc binary.gz > data2.bz2 
bzip2 -dc data2.bz2 > data3.gz
gzip -dc data3.gz > data4.tar
tar -xOf data4.tar > data5.tar
tar -xOf data5.tar > data6.bz2
bzip2 -dc data6.bz2 > data7.tar
tar -xOf data7.tar > data8.gz
gzip -dc data8.gz > data9.txt
```

Last step is to print the `data9.txt` to stdout.

```bash
cat data9.txt
```

This is the final one-liner:

```bash
xxd -r data.txt > binary.gz && gzip -dc binary.gz > data2.bz2 && bzip2 -dc data2.bz2 > data3.gz && gzip -dc data3.gz > data4.tar && tar -xOf data4.tar > data5.tar && tar -xOf data5.tar > data6.bz2 && bzip2 -dc data6.bz2 > data7.tar && tar -xOf data7.tar > data8.gz && gzip -dc data8.gz > data9.txt && cat data9.txt
```

**Flag:** `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn` 

**Takeaway:** In this challenge we learned about different commands (`gzip`, `bzip2`, `tar`) to compress and decompress files. Use the `man` command to learn more about them.

---

## Lvl 13 → 14

**Goal:** use the **private key** in the home directory to log in to the next level and read the password.  

On login, list the files in the home directory with `ls`

```bash
ls
```

Then print the contents of `sshkey.private` and copy them.

```bash
cat sshkey.private
```

Now exit from the **ssh session** with the `exit` command. 
On your local machine, create a file `privatekey` and put the content inside.

```bash
echo "-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==
-----END RSA PRIVATE KEY-----" > privatekey
```

Now we have to log in to **bandit14** using the **privatekey** file we created, but before that we need to set the right **permissions** on the `privatekey` file.

```bash
chmod 600 privatekey
```

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220 -i privatekey
```

Now that we are inside, let's `cat` the bandit14's password.

```bash
cat /etc/bandit_pass/bandit14
```

**Flag:** `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS` 

**Takeaway:** In this level, we learned how to use the `ssh` command with a **private key**, which greatly increases security on our servers. 

---

## Lvl 14 → 15

**Goal:** Submit the current level's password to `localhost` on port `30000`. 

The first thing that comes to mind is to scan port `30000` using `nmap`.

```bash
nmap localhost -p30000
```

Now we know that the port is **open**, the `ndmps` service is running and the internet protocol used is tcp.
Let's try the `nc` command to send the current password to that port.

```bash
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

**Flag:** `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`  

**Takeaway:** In this challenge we learned how to use `netcat` to send a message to a service given the **IP** and **port**.

---

## Lvl 15 → 16

**Goal:** Submit the current level's password to `localhost` on port `30001` using SSL/TLS encryption. 

Here we can use the `openssl s_client` command to submit this level's password to **localhost:30001** (remember that the password is located at `/etc/bandit_pass/bandit15`).

```bash
cat /etc/bandit_pass/bandit15 | openssl s_client -quiet -connect localhost:30001
```

**Flag:** `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx` 

**Takeaway:** In this level we learned how to use the `openssl s_client` command to send an encrypted message between client (us) and server (in this case also us, but this is not always the case).

---

## Lvl 16 → 17

**Goal:** Submit the current level's password to `localhost` on one of the ports from `31000` to `32000` using SSL/TLS encryption (if the server responds with the current level's password, that means you hit the wrong port). 

Here the first objective is to find the correct port to send the **message** to, to do so, we'll use the `nmap` command to scan a range of ports on `localhost`.

```bash
nmap localhost --script ssl-cert -p31000-32000
```

After the scan this will be the result:

```bash
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
| ssl-cert: Subject: commonName=SnakeOil
| Issuer: commonName=SnakeOil
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-10T03:59:50
| Not valid after:  2034-06-08T03:59:50
| MD5:   fa04:c746:b0d0:c3a1:984c:0708:ed4c:4505
|_SHA-1: 323a:f3b1:4fc7:1b0f:f71a:1931:8ff3:62a1:49ac:735a
31691/tcp open  unknown
31790/tcp open  unknown
| ssl-cert: Subject: commonName=SnakeOil
| Issuer: commonName=SnakeOil
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-10T03:59:50
| Not valid after:  2034-06-08T03:59:50
| MD5:   fa04:c746:b0d0:c3a1:984c:0708:ed4c:4505
|_SHA-1: 323a:f3b1:4fc7:1b0f:f71a:1931:8ff3:62a1:49ac:735a
31960/tcp open  unknown
```

Now that we have identified the possible **ports**, let's use the `openssl s_client` command to send the current level's password to `localhost:port`, by trying each port that has SSL/TLS.

```bash
cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31518
```
The first attempt gives us back the current level's password so it's not correct. Let's try the other port.

```bash
cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31790
```

This time the **output** is a **Private Key**, this must be the solution to this level!

To make use of the **Private Key**, let's save it to a file named `bandit17_privatekey`.

```bash 
echo "-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----" > bandit17_privatekey
```

The last step is to give the right permissions to the private key.

```bash
chmod 600 bandit17_privatekey
```

**Flag:** The **private key** saved in this file `bandit17_privatekey` 

**Takeaway:** In this level we learned how to use the `nmap` command to scan for open ports on a server, we also used the `--script` option to analyze the ports for `SSL/TLS` encryptions, after that we used the `openssl s_client` command to send a message with the current level's password.

---

## Lvl 17 → 18

**Goal:** Find the only line that has been changed in the `passwords.new` compared to `passwords.old`.

To compare two files line by line, we can use the `diff` command, the flag will be the text preceded by `>`.

```bash
diff passwords.old passwords.new
```

**Flag:** `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO` 

**Takeaway:** In this level we have learned how to use the `diff` to detect changed content line by line. 

---

## Lvl 18 → 19

**Goal:** Print the `readme` file in the home directory, the only problem is that the `.bashrc` file was modified to log you out as soon as you log in with SSH. 

To bypass the log out problem, we can specify the command to run directly when connecting to the machine with SSH.

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

**Flag:** `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8` 

**Takeaway:** In this challenge we learned that we can specify a command to run directly on connection using `ssh`, in this case we print the `readme` file that we knew the existence of but in other cases we could have used the `ls` command before the `cat` command.

---

## Lvl 19 → 20

**Goal:** To access the next level you have to use the `bandit20-do` binary in the home directory. 

First step is to run the `./bandit20-do` command to see what it does.

```bash
./bandit20-do
```

The binary suggests to try executing the command with the `whoami` argument.

```bash
./bandit20-do whoami
```

The output is `bandit20`, this means that any command passed as argument to `./bandit20-do` will be executed as `bandit20` user. Let's try to print the password on `bandit20`.

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

It seems to work. We have found our flag!

**Flag:** `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO` 

**Takeaway:** In this challenge we used a **setuid** binary, which runs with the privileges of its owner rather than the user executing it, allowing us to read files accessible only to bandit20. 

---

## Lvl 20 → 21

**Goal:** In this level, we can find a binary in the home directory. The binary connects to a given localhost port, reads the text from the connection, and responds with the **bandit21** password if the input matches the **bandit20** password. 

The first step is to run the `./suconnect` binary to see the usage.

```bash

./suconnect
```

This is the output that tells us something more about the level.

```bash
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
```

Let's try to set up a simple server in background sending the current level's password on an **arbitrarily chosen** port with the `nc` command.

```bash
cat /etc/bandit_pass/bandit20 | nc -lnp 3333 &
```

After setting the server up, let's run the `./suconnect` command with the same port we used for the listener to get the next password.

```bash
./suconnect 3333
```

**Flag:** `EeoULMCra2q0dSkYj561DX7s1CpBuOBt` 

**Takeaway:** In this level, we learned how to set up a listening server in background with the `nc` command and the `&` symbol. 

---

## Lvl 21 → 22

**Goal:** We have to look in `/etc/cron.d/` folder for commands that are executed regularly to solve this level.  

The first step is to change directory into the `/etc/cron.d` directory and list its content. 

```bash
cd /etc/cron.d
ls 
```

Among the listed files we can spot `cronjob_bandit22`, let's print its content.

```bash
cat /etc/cron.d/cronjob_bandit22
```

After printing the `/etc/cron.d/cronjob_bandit22` file we can see that it sends the **bandit22 password** to a temporary file, let's see if we have access to that file. 

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

**Flag:** `tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q` 

**Takeaway:** In this level, we learned how to read time-based job schedules, by going to the `/etc/cron.d/` folder.

---

## Lvl 22 → 23

**Goal:** This level is similar to the one before, there is a cron job running, we have to analyze the script to uncover the password.

As before, the first step is to change directory to `/etc/cron.d` and list its content.

```bash
cd /etc/cron.d
ls
```

Here we can find a file named `cronjob_bandit23`, let's print it.

```bash
cat cronjob_bandit23
```

From the output of this file we understand that it runs a shell script, let's analyze the script by printing it to the screen.

```bash
cat /usr/bin/cronjob_bandit23.sh
```

By reading the script we can understand that the code copies the **bandit23 password** to a temporary filename using this piece of code `echo I am user $myname | md5sum | cut -d ' ' -f 1`, let's try running it by replacing `$myname` with `bandit23` to uncover the filename.

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

The output of this command will be this: `8ca319486bfbbc3663ea0fbe81326349`, let's try to print the content of that file in the `/tmp/` folder.

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

**Flag:** `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga` 

**Takeaway:** In this level, we learned how to read a cron script and replicate part of it to uncover a password-protected filename. 

---

## Lvl 23 → 24

**Goal:** This level is similar to the one before, there is a cron job running, but this time we have to create a script of our own.

As before, the first step is to change directory to `/etc/cron.d` and list its content.

```bash
cd /etc/cron.d
ls
```

Here we can find a file named `cronjob_bandit24`, let's print it.

```bash
cat cronjob_bandit24
```

From the output of this file we understand that it runs a shell script, let's analyze the script by printing it to the screen.

```bash
cat /usr/bin/cronjob_bandit24.sh
```

By reading the script we find out that it runs all the scripts inside this folder `/var/spool/bandit24/foo` owned by `bandit23` and then it deletes the script, so let's make a temporary folder to work with and change directory into it.

```bash
mktemp -d
cd /tmp/tmp.your_random_folder
```

Now let's create a script that can be executed, this can be done with `vim`, `nano` or `echo`.

```bash 
vim script.sh
```

This will be the script, at the top we put the `shebang` and below it are the commands to be executed.


```bash
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/tmp.your_random_folder/password
```

Now let's set the right permissions to the script and also to the temp folder that is accessible only to us right now.

```bash
chmod 777 script.sh
chmod 777 .
```

Now the last step is to copy the script to the right folder that we discovered before, and spam the `ls` command inside the temp folder until the `password` file appears.

```bash
cp ./script.sh /var/spool/bandit24/foo
```

When the `password` file appears, just print it.

```bash 
cat password
```

**Flag:** `gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8` 

**Takeaway:** In this level we learned how to create our first **script** and how to run it by setting the right permissions. 

---

## Lvl 24 → 25

**Goal:** The goal of this level is to brute force a 4-digit pincode that must be sent with the bandit24 password to localhost port `30002`.

The first step is to create a temporary folder to work with, and change directory to it.

```bash
cd $(mktemp -d)
```
Now before creating the script let's try sending the bandit24 password with pincode `0000` just to see the response.

```bash
echo "$(cat /etc/bandit_pass/bandit24) 0000" | nc localhost 30002
```

One thing that we can notice is that the response gives us the format that is password + space + pincode and we also notice that when the pincode is wrong the answer contains the keyword `Wrong!`, let's write down a brute-forcing script called `brute-force`.

```bash
vim brute-force
```

We will start with the `shebang` and the rest will be a for loop that loops all the numbers from `0000` to `9999` combined with the **bandit24 password** and a space.

```bash
#!/bin/bash

password=$(cat /etc/bandit_pass/bandit24)

for pin in {0000..9999}; do
    echo "$password $pin"
done | nc -N localhost 30002 | grep -v "Wrong"
```

Now let's give the `brute-force` script the right permissions.

```bash
chmod 700 brute-force
```

The last step is to execute the script and copy the password for the next level.

```bash
./brute-force
```

**Flag:** `iCi86ttT4KSNe1armKiwbQNmB3YJP3q4` 

**Takeaway:** In this level we wrote a more complex script with the goal of brute forcing a 4-digit pin, this is a real plausible attack.

---

## Lvl 25 → 26

**Goal:** In this level we are provided with the `bandit26.sshkey` private key, the only problem is the shell set for **bandit26**. 

The first step is to save the private key on our host, just print it to the screen, copy it and save it on our machine with the right permissions.

```bash
cat bandit26.sshkey
```

Copy the text, exit from the remote machine and save the file on our machine.

```bash
echo "-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEApis2AuoooEqeYWamtwX2k5z9uU1Afl2F8VyXQqbv/LTrIwdW
pTfaeRHXzr0Y0a5Oe3GB/+W2+PReif+bPZlzTY1XFwpk+DiHk1kmL0moEW8HJuT9
/5XbnpjSzn0eEAfFax2OcopjrzVqdBJQerkj0puv3UXY07AskgkyD5XepwGAlJOG
xZsMq1oZqQ0W29aBtfykuGie2bxroRjuAPrYM4o3MMmtlNE5fC4G9Ihq0eq73MDi
1ze6d2jIGce873qxn308BA2qhRPJNEbnPev5gI+5tU+UxebW8KLbk0EhoXB953Ix
3lgOIrT9Y6skRjsMSFmC6WN/O7ovu8QzGqxdywIDAQABAoIBAAaXoETtVT9GtpHW                                      qLaKHgYtLEO1tOFOhInWyolyZgL4inuRRva3CIvVEWK6TcnDyIlNL4MfcerehwGi
il4fQFvLR7E6UFcopvhJiSJHIcvPQ9FfNFR3dYcNOQ/IFvE73bEqMwSISPwiel6w
e1DjF3C7jHaS1s9PJfWFN982aublL/yLbJP+ou3ifdljS7QzjWZA8NRiMwmBGPIh                                      Yq8weR3jIVQl3ndEYxO7Cr/wXXebZwlP6CPZb67rBy0jg+366mxQbDZIwZYEaUME
zY5izFclr/kKj4s7NTRkC76Yx+rTNP5+BX+JT+rgz5aoQq8ghMw43NYwxjXym/MX
c8X8g0ECgYEA1crBUAR1gSkM+5mGjjoFLJKrFP+IhUHFh25qGI4Dcxxh1f3M53le
wF1rkp5SJnHRFm9IW3gM1JoF0PQxI5aXHRGHphwPeKnsQ/xQBRWCeYpqTme9amJV
tD3aDHkpIhYxkNxqol5gDCAt6tdFSxqPaNfdfsfaAOXiKGrQESUjIBcCgYEAxvmI
2ROJsBXaiM4Iyg9hUpjZIn8TW2UlH76pojFG6/KBd1NcnW3fu0ZUU790wAu7QbbU
i7pieeqCqSYcZsmkhnOvbdx54A6NNCR2btc+si6pDOe1jdsGdXISDRHFb9QxjZCj
6xzWMNvb5n1yUb9w9nfN1PZzATfUsOV+Fy8CbG0CgYEAifkTLwfhqZyLk2huTSWm
pzB0ltWfDpj22MNqVzR3h3d+sHLeJVjPzIe9396rF8KGdNsWsGlWpnJMZKDjgZsz
JQBmMc6UMYRARVP1dIKANN4eY0FSHfEebHcqXLho0mXOUTXe37DWfZza5V9Oify3
JquBd8uUptW1Ue41H4t/ErsCgYEArc5FYtF1QXIlfcDz3oUGz16itUZpgzlb71nd                                      1cbTm8EupCwWR5I1j+IEQU+JTUQyI1nwWcnKwZI+5kBbKNJUu/mLsRyY/UXYxEZh
ibrNklm94373kV1US/0DlZUDcQba7jz9Yp/C3dT/RlwoIw5mP3UxQCizFspNKOSe
euPeaxUCgYEAntklXwBbokgdDup/u/3ms5Lb/bm22zDOCg2HrlWQCqKEkWkAO6R5
/Wwyqhp/wTl8VXjxWo+W+DmewGdPHGQQ5fFdqgpuQpGUq24YZS8m66v5ANBwd76t                                      IZdtF5HXs2S5CADTwniUS5mX1HO9l5gUkk+h0cH5JnPtsMCnAUM+BRY=
-----END RSA PRIVATE KEY-----" > bandit26.sshkey
```

Give it the right permissions.

```bash
chmod 600 bandit26.sshkey 
```

Now let's log in to the next level using the private key.

```bash
ssh bandit26@bandit.labs.overthewire.org -p 2220 -i bandit26.sshkey
```

Once we try to log in, the shell kicks us out, let's try reading the shell set for the bandit26 user by logging in to bandit25.

```bash
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

To read the shell set for the bandit26 user we can just print the `/etc/passwd` file and grep for the bandit26 user.

```bash
cat /etc/passwd | grep "bandit26"
```

By printing the bandit26 `passwd` info we find out that this is the shell set for the user: `/usr/bin/showtext`, let's print the content to the screen and try to analyze it.
    
```bash
cat /usr/bin/showtext
```

It seems like when we log in the `more` command is executed, let's check how long the `text.txt` file is, because `more` only pauses for input when the content is taller than the terminal.

```bash
cat /home/bandit26/text.txt
```

We don't have access to that file, but we can force `more` into interactive mode by making our terminal window **shorter than the file content**, so let's log in to bandit26 with a **smaller terminal size** (literally by reducing the number of rows of our terminal window).


```bash
ssh bandit26@bandit.labs.overthewire.org -p 2220 -i bandit26.sshkey
```

If the window is small enough `more` will pause for input, now that we are inside the `more` command let's press `v` to enter `vim` mode where we can execute commands.

Now we can execute a shell from vim, first let's set the shell, then run it.

```bash
:set shell=/bin/bash
:shell
```

**Flag:** No password flag for this level; the SSH key provided in bandit25's home directory is the credential. The goal is to gain a working shell as bandit26.

**Takeaway:** This level required some knowledge about the `more` command and also about the `vim` commands, I would highly suggest learning about `vim` and `more` commands in detail, one tip is to use `vim` as your daily editor, you will uncover a great power.

---

## Lvl 26 → 27

**Goal:** Good job getting a shell! Now grab the password for bandit27.

Re-enter bandit26 using the same trick from the previous level (shrink the terminal, SSH in, `more` pauses, press `v`, then run `:set shell=/bin/bash` and `:shell`).

Now that we have a shell as bandit26, let's list the home directory to see what's available.

```bash
ls
```

We can see a `bandit27-do` setuid binary, just like in level 19→20. Let's use it to read the bandit27 password.

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

**Flag:** `upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB` 

**Takeaway:** Nothing crazy to learn here, just be proud of the effort of the previous level. 

---

## Lvl 27 → 28

**Goal:** There is a git repository at `ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo`. The password for the user `bandit27-git` is the same as for the user bandit27. (Note that this level is meant to be done on the local machine, and make sure to have `git` installed).

For this walkthrough we will operate from our local machine, let's make a temporary folder and clone the repo.

```bash
cd $(mktemp -d)
```

We will use the `git clone` command to clone the repo, we can specify the port with `:port` at the end of the domain, and when prompted enter the `bandit27-git` user password, which is the same as for the bandit27 user.

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

Once the repository is cloned, we will see a `repo` folder inside our temporary directory.

```bash
ls
```

Just enter the folder and list its contents.

```bash
cd repo
ls
```

Inside there is only one file, print it.

```bash 
cat README
```

**Flag:** `Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN` 

**Takeaway:** In this level we learned how to clone a repository with `git`, how to specify a `port`, how to navigate the cloned repository with standard Linux commands. 

---

## Lvl 28 → 29

**Goal:** There is a git repository at `ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo`. The password for the user `bandit28-git` is the same as for the user bandit28. (Note that this level is meant to be done on the local machine, and make sure to have `git` installed).

In this level as in the one before we will use a temporary folder on our local machine.

```bash
cd $(mktemp -d)
```

The next step is to clone the repo, you should know how to do it from the previous level.

```bash
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
```

Now let's enter the `repo` folder and list its contents.

```bash
cd repo
ls
```

There is a `README.md` file, let's print it.

```bash
cat README.md
```

We can notice that the password is now hidden from us, let's try to go back one version using the power of git version control.

The first step is to take a look at the logs.

```bash
git log
```

Doing so, we can see that there is a `fix info leak` commit. Let's go back to the commit before that by copying its hash from the `git log` output.


```bash
git reset --hard a3437bddd447f2d496731658e86b98cbea9d3c98
```

Now that we have an older version, let's print `README.md`.

```bash
cat README.md
```

**Flag:** `4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7` 

**Takeaway:** In this level we learned how to check logs with `git`, and how to reset to a previous commit (aka moving the HEAD). 

---

## Lvl 29 → 30

**Goal:** There is a git repository at `ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo`. The password for the user `bandit29-git` is the same as for the user bandit29. (Note that this level is meant to be done on the local machine, and make sure to have `git` installed).

In this level as in the ones before we will use a temporary folder on our local machine.

```bash
cd $(mktemp -d)
```

The next step is to clone the repo, you should know how to do it from the previous level.

```bash
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
```

Now let's enter the `repo` folder and list its contents.

```bash
cd repo
ls
```

There is a `README.md` file, let's print it.

```bash
cat README.md
```

The file tells us there are no passwords in production, so the password must be on a different branch. Let's check the logs first.

```bash
git log
```

After checking the logs, nothing relevant seems to pop out. Let's list all refs including remote branches with `git branch -a`: refs are named pointers to commits, and branches are a type of ref; remote branches like `origin/dev` won't show up with a plain `git log`.

```bash
git branch -a
```

Now we can see the existence of a `dev` branch, let's switch to it.

```bash
git checkout dev
```

Now let's print `README.md` to uncover the flag.

```bash
cat README.md
```

**Flag:** `qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL` 

**Takeaway:** In this level we learned that refs are named pointers to commits in git: branches, tags, and `HEAD` are all refs. Remote branches like `origin/dev` are also refs and can be listed with `git branch -a`. We can switch to them with `git checkout <branch-name>`.

---

## Lvl 30 → 31

**Goal:** There is a git repository at `ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo`. The password for the user `bandit30-git` is the same as for the user bandit30. (Note that this level is meant to be done on the local machine, and make sure to have `git` installed).

In this level as in the ones before we will use a temporary folder on our local machine.

```bash
cd $(mktemp -d)
```

The next step is to clone the repo, you should know how to do it from the previous level.

```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
```

Now let's enter the `repo` folder and list its contents.

```bash
cd repo
ls
```

Here we find a `README.md` file, let's print it.

```bash
cat README.md
```

Nothing seems to be inside it, let's do a quick log and branch check.

```bash
git log
git branch -a
```

Nothing unusual. Let's think about it: if it isn't in a file or branch, what else could it be? One thing that comes to mind is tags, let's check them with the `git tag` command.

```bash
git tag
```

Now that we know about the existence of the `secret` tag, let's try reading its message with `git show`.

```bash 
git show secret
```

There is our flag.

**Flag:** `fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy` 

**Takeaway:** In this level we learned about the existence of `tags` in git, how to list and read their messages. 

---

## Lvl 31 → 32

**Goal:** There is a git repository at `ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo`. The password for the user `bandit31-git` is the same as for the user bandit31. (Note that this level is meant to be done on the local machine, and make sure to have `git` installed).

In this level as in the ones before we will use a temporary folder on our local machine.

```bash
cd $(mktemp -d)
```

The next step is to clone the repo, you should know how to do it from the previous level.

```bash
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
```

Now let's enter the `repo` folder and list its contents.

```bash
cd repo
ls
```

There is a `README.md` file, let's print it.

```bash
cat README.md
```

This time our goal is to push a file named `key.txt` that should contain `May I come in?` on the `master` branch.

The first step is to create the file.

```bash
echo "May I come in?" > key.txt
```

Now let's add the file to the version control.

```bash 
git add key.txt
```

Git warns us that `key.txt` is being ignored due to a `.gitignore` rule. Let's open `.gitignore` to inspect it.

```bash
vim .gitignore
```

In the `.gitignore` file all `*.txt` files are ignored, let's remove that line.
Now we can add the `key.txt` file to the version control.

```bash
git add key.txt
```

Now let's create a commit.

```bash
git commit -m "key"
```

The last step is to push the commit.

```bash 
git push
```

This will ask us to enter the current level's password.
The push will be rejected, but we'll get the flag.

**Flag:** `3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K` 

**Takeaway:** In this level we learned how to add a file to the version control, how to commit a change, how to push a commit to a remote repository and how the `.gitignore` file works. 

---

## Lvl 32 → 33

**Goal:** This level is another escape from a shell. 

Let's log in using the previous level password.

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

Once we log in, we are welcomed with this message `WELCOME TO THE UPPERCASE SHELL`, let's try running some commands to get comfortable with the level.

```bash 
ls 
```

It seems that any command we run gives us `Permission denied`, we can also notice the prefix `sh: 1:`. This, for a beginner, could look like nothing, but more advanced people know that `$0` is a special variable that holds the path to the current shell. Running it directly launches a new shell session, bypassing the uppercase conversion since `$0` is not a regular alphabetic command.

```bash
$0
```

Now we have a shell, let's print the password for the next level.

```bash
cat /etc/bandit_pass/bandit33
```

**Flag:** `tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0` 

**Takeaway:** In this escape level, the trick was the knowledge of special shell parameters. `$0` holds the path to the current shell and is not a regular alphabetic command, so it bypasses the uppercase filter entirely. 

---

## Lvl 33 → 34

**Goal:** Just SSH into the level to confirm you have completed the game.

```bash
ssh bandit33@bandit.labs.overthewire.org -p 2220
```

Now read the `README.txt` file, you should be able to do it with no help from me :P.

**Flag:** `no flag is provided for the last level`

**Takeaway:** Congratulations on completing all the bandit levels! 

## Conclusion 

First of all, congratulations! Completing all the levels is not an easy task, it requires discipline and commitment. I hope you learned something new, even if I didn't explain each command in detail, and that is intentional: **no expert CLI user knows all the commands and their options. Real experts use logic and the `man` command to solve any problem.**

Each level was written and personally tested by me. I'm from Italy and English is not my native language; grammar errors were corrected with the help of Claude Code.

Follow me on X: [granatavalentin](https://x.com/granatavalentin)
