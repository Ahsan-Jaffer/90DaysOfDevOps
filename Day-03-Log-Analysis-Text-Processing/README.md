# Day 3 — Linux Log Analysis and Text Processing

## Overview

Today I practiced one of the most practical parts of Linux for DevOps: **log analysis**.

The main goal was to understand how a DevOps engineer reads server logs, finds useful information, cleans the output, removes duplicate data, and shares a clear report with developers.

This was not just command practice. It was a real DevOps troubleshooting workflow.

---

## What I Learned Today

### 1. File Ownership in Linux

I learned that Linux files and directories have owners and groups. We can check ownership using:

```bash
ls -l
```

I also practiced changing the owner of a file using:

```bash
sudo chown jaffer commands.txt
```

**Key point:** `chown` means **change owner**.

This is useful when a file or directory belongs to the wrong user and another user or service needs access to it.

---

### 2. Why Log Files Are Important

I learned that servers store important activity inside log files.

A log file can tell us:

- When an event happened
- Which service generated the event
- Which user was involved
- Which IP or host connected to the server
- Whether the request was successful or failed
- What kind of error occurred

In DevOps, logs are important because they help us troubleshoot real system issues with evidence instead of guessing.

---

### 3. NGINX Log Files

I explored NGINX log files inside:

```bash
/var/log/nginx
```

The main files were:

```bash
access.log
error.log
```

`access.log` stores requests coming to the web server.

Example information from access logs:

- Client IP
- Date and time
- Requested URL
- HTTP method
- Status code
- Browser or user-agent

Common HTTP status codes:

| Status Code | Meaning |
|---|---|
| 200 | Request successful |
| 301 / 302 | Redirect |
| 403 | Forbidden |
| 404 | Page or file not found |
| 500 | Internal server error |
| 502 | Bad gateway |
| 503 | Service unavailable |

---

### 4. grep — Searching Patterns in Files

I learned that `grep` is used to search for a pattern inside a file.

```bash
grep "authentication failure" app.log
```

This command searches for all lines containing `authentication failure`.

I also practiced:

```bash
grep -i -n "authentication failure" app.log | head -n 4
```

| Part | Meaning |
|---|---|
| grep | Search text inside a file |
| -i | Ignore uppercase/lowercase difference |
| -n | Show line numbers |
| "authentication failure" | Search pattern |
| app.log | File name |
| head -n 4 | Show only the first 4 results |

---

### 5. head and tail — Viewing File Output

I learned how to view the beginning and ending lines of a file.

```bash
head app.log
head -n 5 app.log
tail app.log
tail -n 5 app.log
tail -f app.log
```

`tail -f` is very useful for live troubleshooting because it shows new log lines in real time.

---

### 6. awk — Extracting Useful Data

I learned that `awk` is a command-line tool with programmatic behavior.

It can read a line, split it into columns, and print specific fields.

```bash
awk '/authentication failure/ {print $1,$2,$3}' app.log
```

This command prints the date and time from lines containing `authentication failure`.

| Field | Meaning |
|---|---|
| $1 | First column |
| $2 | Second column |
| $3 | Third column |
| $0 | Full line |
| NF | Number of fields |
| NR | Current line number |

Example use case:

```bash
awk '/authentication failure/ {print $1,$2,$3}' app.log
```

This helps extract only the useful data instead of reading the full log manually.

---

### 7. sed — Replacing and Cleaning Text

I learned that `sed` is a stream editor.

It is mostly used to replace, clean, or modify text output.

```bash
sed "s/rhost=/IP=/g" auth_failure_ips.txt
```

This replaces `rhost=` with `IP=`.

```bash
sed "s/user=/username=/g" auth_failure_ips.txt
```

This replaces `user=` with `username=`.

Combined command:

```bash
sed "s/rhost=/IP=/g; s/user=/username=/g" auth_failure_ips.txt
```

By default, `sed` changes only the output shown in the terminal. It does not modify the original file unless we use `-i`.

---

### 8. sort and uniq — Removing Duplicate Data

Raw logs often contain duplicate data.

Instead of sending repeated lines to a developer, a DevOps engineer should clean the output and send only useful information.

```bash
sort -u auth_failure_ips.txt
```

This sorts the file and removes duplicate lines.

Another method:

```bash
sort auth_failure_ips.txt | uniq
```

Important point: `uniq` only removes duplicate lines that are next to each other. That is why using `sort` before `uniq` is important.

---

### 9. Removing Blank IP Lines

While cleaning data, I saw blank values like:

```bash
IP=
```

To remove this type of line, I used filtering logic:

```bash
sed "s/rhost=/IP=/g; s/user=/username=/g" auth_failure_ips.txt | grep -v "^IP=$" | sort -u
```

This removes blank IP entries and keeps only useful unique records.

---

### 10. find — Searching Files and Directories

I learned that `find` is used to locate files and directories.

```bash
find . -name "app.log"
```

| Part | Meaning |
|---|---|
| find | Search for files/directories |
| . | Start searching from the current directory |
| -name | Search by name |
| "app.log" | File name |

Other useful examples:

```bash
find . -name "*.log"
find . -type f -name "*.log"
```

---

## Practical DevOps Workflow Practiced Today

Today I practiced this workflow:

```text
Find the log file
Search for the error
Extract useful fields
Clean the output
Remove duplicate data
Prepare a useful report
Share it with the developer
```

This is how DevOps supports development teams in real troubleshooting.

---

## Commands Practiced Today

```bash
ls -l
sudo chown jaffer commands.txt
cat app.log
grep -i -n "authentication failure" app.log | head -n 4
head -n 5 app.log
tail -n 5 app.log
tail -f app.log
awk '/authentication failure/ {print $1,$2,$3}' app.log
sed "s/rhost=/IP=/g; s/user=/username=/g" auth_failure_ips.txt
sort -u auth_failure_ips.txt
sort auth_failure_ips.txt | uniq
find . -name "app.log"
find . -type f -name "*.log"
```

---

## DevOps Collaboration Lesson

Today I understood that DevOps collaboration is not only about talking to developers.

Real collaboration means giving developers clear and useful information.

A weak report would be:

```text
There is an error.
```

A better DevOps report would be:

```text
Authentication failure happened at this date and time.
This user was affected.
This IP or host was involved.
Here is the cleaned log report.
```

This helps developers debug faster and solve issues with real evidence.

---

## Key Takeaway

Commands are not the goal.

Problem-solving is the goal.

Today I learned how Linux commands help DevOps engineers analyze logs, extract useful information, clean data, remove duplicates, and support developers in troubleshooting real issues.

---

## Commands Summary

| Command | Purpose |
|---|---|
| grep | Search matching patterns inside files |
| awk | Extract useful columns/data from text |
| sed | Replace and clean text |
| head | View the first lines of a file |
| tail | View the last lines or live logs |
| sort | Sort lines in order |
| uniq | Remove duplicate adjacent lines |
| find | Locate files and directories |
| chown | Change file or directory ownership |

---

## Next Topic

The next topic will be **Linux networking commands**, including commands used to check connectivity, IP addresses, DNS, ports, and server responses.
