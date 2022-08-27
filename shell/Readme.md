# Ceyshell
_____
ceyshell is a command line interface written in c++, It supports basic and fundamental functionalities. But since its purpose is for demonstration and not for the professinal use, it has many limitations.
<br><br>

___
## Executing Cell
To run the command line proceed with the following instruction.

 Compile ceyshell.cpp and run using the following command:
```
g++ ceyshell.cpp -o ceyshell && ./ceyshell
```

---

## Functionality of the Ceyshell
Supports following built-in commands:
* cd \<dir>  : to change the present working directory<br>
<img src = "https://raw.githubusercontent.com/brillard1/brillard/main/shell/png/cd.png">
    
* history [n] : returns previous [n] commands executed<br>
 <img src = https://raw.githubusercontent.com/brillard1/brillard/main/shell/png/history.png>
    
* help<br>
 <img src = "https://raw.githubusercontent.com/brillard1/brillard/main/shell/png/help.png">
    
* exit<br>
<img src = "https://raw.githubusercontent.com/brillard1/brillard/main/shell/png/exit.png">

* Supports command concatenation using `&&`<br>
<img src = "https://raw.githubusercontent.com/brillard1/brillard/main/shell/png/and_seg.png">

* Supports foreground and background processes<br>
<img src = "https://raw.githubusercontent.com/brillard1/brillard/main/shell/png/wait.png">

<br>

## Limitations during implementation
<ol>
<li>executing <code>cd</code> without the argument does not change the directory to <code>/home</code>
<li><code>&&</code> can only pipe two commands at a time
<li>auto-completion toolkit is not supported but can be implemented with already existing toolkits.
<li>supports history but does not support direct selection of previous commands using upper arrow
</ol>

## Salient Features
* csh_loop while allocating memory for the inputs takes excellent care that buffer overflow does not happen and reallocates more memory to it, as and when needed. 
(However this functionality for `|` and `&&` commands in not yet available)


## Member methods
1. get_working_path: prints the current working directory in the terminal
2. csh_cd : for directory navigation; implemented using chdir
3. csh_help: returns basic command guidelines
5. csh_num_builtins: returns number of builtin commands we have defined
6. csh_history
    * return history of previously run commands
    * if no argument is passed, it returns previous 100 commands.
    1. appendLineToFile: this is implemented using appendLineToFile fucntion
        * is creates `history.txt` file in the directory our shell is present, if it does not exist, and appends the command we run in `ceyshell` at the end of it.
        * last N lines of the file are extracted using an implementation of circular queue. 
7. csh_read_line: to read the input from the user; takes special care in error handling and avoiding buffer overflow
8. csh_split_line : splits our input as per `\t`, `\n`, `\r`, `\a`
9. csh_loop
    1. csh_loop
        * Runs the shell continously, displaying the current `PWD` followed by a `$` while waiting for the user input. 
        * it recieves the input from user using `csh_read_line`, tokenizes it using `csh_split_line`, appends the command in history 
        * it also checks if the entered command has `&&` or `|` in it and bifurcates the command into two.
        * having tokenized the commands properly, it then calls `csh_execute` on it.
    2. csh_execute
        * it initially checks the validity of our entered commands, compares them with the builtin commands, executes them on a match
        * Else it calls csh_launch
    3. csh_launch
        * it forks and creates a child process. Depending upon the return value of fork, it takes logical decisions. A return value greater than 0 is indicative of parent process and therefore, it waits for the child_process to complete. 
        * however, if our command ends in `&`, the control does not wait and our process continues to run in the background as our shell become ready to accept new input from the user.
10. main 
    * entry point of our program
    * calls csh_loop
