# Linux-Redirection
Five minute lesson on Linux Redirection

### What is Redirection?

Redirection is a fundamental concept in Linux that enables you to control where commands obtain their input and where they direct their output. By default, when you run a command in Ubuntu, it reads input from the keyboard (stdin) and displays output on the terminal screen (stdout). Redirection allows you to modify this behavior to read from files, write to files, or execute commands in sequence.

### The Three Standard Streams
Ubuntu uses three standard data streams for every command:

Stream | Description | File Descriptor | 
--- | --- | --- | 
stdin | Standard input (keyboard) | 0 |
stdout | Standard output (screen) | 1 |
stderr | Standard error (screen) | 2 |

### Basic Redirection Operators

#### Output Redirection (>)
The > oeperator redirects standard output to a file (overwrites existing files):
```bash
#Save directory listing to a file
ls -l > ls_ouptut.txt

#save date to a file
date > datetime.txt
```

#### Append Redirection (>>)
Use >> to append output to an existing file without overwriting:
```bash
#append to existing file
echo "New line" >> existing.txt

#append command output
date >> datetime.txt
```

#### Input Redirection (<)
The < operator redirects input from a file instead of keyboard:
```bash
#read from file instead of keyboard
cat < input.txt

#sort contents fo a file
sort < names.txt
```
#### Error Redirection
Redirecting Standard Error (2>)
Error messages go to stderr (file descriptor 2).  You can redirect errors separately.
```bash
#redirect errors to a file
find/etc -name "*.conf" 2> errors.txt

#discard error messages
command 2> /dev/null
```
#### Combining Output and Error
To redirect both stdout and stderr to the same file:
```bash
#method 1: redirect stdout, then stderr to stdout
command > output.txt 2>&1

#method 2: bash shortcut (modern syntax)
command &> output.txt
```
#### Pipes (|)
Pipes connect the output of one command to the input of another:
```bash
#basic pipe
ls -l | grep '.txt'

#chain multiple commands
ps aux | grep 'python' | head -2

#count files in directory
ls -1 | wc -l
```
#### Special Files and Devices
/dev/null is a special file that discards anything written to it:
```bash
# Suppress all output
command > /dev/null

# Suppress only errors
command 2> /dev/null

# Suppress everything
command > /dev/null 2>&1
```
#### The tee Command
tee displays output on screen and saves it to a file simultaneously:
```bash
# Display and save output
ls -l | tee directory.txt

# Append while displaying
ps aux | tee -a log.txt
```
#### Here Documents (<<)
Here documents allow you to input multiple lines directly in a script:
```bash
# Basic here document
cat << EOF
This is line 1
This is line 2
This is line 3
EOF

# Save to file
cat << EOF > myfile.txt
Content line 1
Content line 2
EOF
```
#### Advanced Examples
```bash
# Monitor system processes and log them
ps aux | tee processes.txt | grep "python"

# Find files and handle errors
find /home -name "*.txt" 2>/dev/null | head -10

# Create a backup script output
tar -czf backup.tar.gz /home/user 2>&1 | tee backup.log
```
#### Using xargs with Pipes
xargs converts stdin into command arguments:
```bash
# Find and delete files
find . -name "*.tmp" | xargs rm

# Process files one by one
ls *.txt | xargs -n1 wc -l

```
### Best Practices
- Always check file existence before using > to avoid overwriting important files
- Use >> for logs to append rather than overwrite
- Redirect errors appropriately - don't just send them to /dev/null unless you're sure
- Test complex redirections with simple commands first
- Use tee when you need to see output AND save it

### Summary

Redirection in Ubuntu is a powerful feature that allows you to:
- Control where commands get input and send output
- Combine commands using pipes
- Handle errors separately from regular output
- Create efficient data processing pipelines
- Manage log files and system output

Master these concepts and you'll be able to create powerful command-line workflows that automate tasks and process data efficiently. Remember: redirection happens before the command runs, pipes connect commands in sequence, and practice with simple examples before building complex pipelines.
