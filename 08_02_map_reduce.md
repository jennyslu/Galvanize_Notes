# Morning Lecture

## Hadoop

# Morning Breakout

Three streams:
 - stdin,
 - stdout, and
 - stderr.

 stdin is the input, which can accept the stdout or stderr.

 stdout is the primary output, which is redirected with >, >>, or |.

 stderr is the error output, which is handled separately so that any exceptions do not get passed to a command or written to a file that it might break; normally, this is sent to a log of some kind, or dumped directly, even when the stdout is redirected. To redirect both to the same place, use:

 
 When a command is executed via an interactive shell, the streams are typically connected to the text terminal on which the shell is running, but can be changed with redirection, e.g. via a pipeline. More generally, a child process will inherit the standard streams of its parent process.


```bash
cmd > file.txt #redirect stdout to truncated filed
foo > stdout.txt 2> stderr.txt #redirect stdout appending to file
cmd &> file.txt #redirect stdout and stderr to truncated file
cmd >>file.txt 2>&1 #redirect stdout and stderr appending to file
foo > stdout.txt 2> stderr.txt #use 2 to redirect part written to stderr
foo > allout.txt 2>&1 #or all in same file
```

# Afternoon Lecture
