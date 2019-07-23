# HEB CDA Python Lunch and Learn, Session 4

2019-02-08

## Topic: Getting familiar with the command line

### "The shell"
- bash
- cmd.exe and Powershell

### Navigating around
- `pwd`
- `ls`
- `cd`

### Getting help
- `man`

### Moving files around
- `mv` and `cp`
- `rm`
- `*` wildcard

### Editing text
- `nano`

### Connecting to a server
- `ssh`
- `scp`
- `screen`
- PuTTY/FileZilla

### `.bashrc` and `.bash_profile`

### Outputting results to a file

## Prerequisites:

- If you're on a Mac computer, you should be ready to go. To open a terminal just go to the search bar and type in "Terminal".
- On Windows PC, you'll want to install Git for Windows. Once you do this, you should have the Git Bash program installed. Let me know if you have any trouble.

## Challenge

Jeremy Z put up some notes on running a batch script with Impala-Shell at *link removed*. By the end of this class, folks should be completely comfortable with doing this, as well as knowing some additional tricks to make doing this a breeze.

## Notes

### Shell

- A shell is a program that lets you tell the computer what you want it to do, and everything is text.
  - A GUI like Windows or MacOS might be a bit more intuitive, but text is easy to share.
  - If you need to communicate with a remote computer (e.g. our edge node), it can be more straightforward to do it in text.
- Mac/Linux: bash shell. Windows: cmd.exe or Powershell.
  - Git Bash lets us do BASH commands in Windows.
  - There are other shell programs (csh, tsh, zsh, xonch, ...) with varying levels of bells and whistles, but bash is the de-facto standard.
- This will also be called the "Command Line".
- Mac/Linux: `$` Windows `>`.
  - For all the examples below, you do *not* type the `$` sign. 

### `pwd` (Print Working Directory)

- Example`$ pwd`
- Tells you where you are on the computer (list all the folders).
- `~` is "Home". On Mac: `/Users/ONEPASSID`, on Windows: `C:\Users\ONEPASSID`.

### `ls` (LiSt directory)
- Example `$ ls`
- Shows you everything that's in your current folder.
- `man ls` tells you all the things you can call.
- Flags come after `-`, and change how the program runs.
  - `-a` shows "hidden" files.
    - Files and folders whose name starts with `.` are hidden. E.G. `.ssh`.
  - `-l` shows details like file size.
- `ls -al` is the same as `ls -a -l`.
- Case sensitive.
- `ls-al` doesn't work. You need a space before your flags.
- You can query inside folders without having to move to them with `cd`. Example: `ls Documents/HEBwork` from being at `~` will show me what's in my Documents->HEBwork folder.
- Windows lest you also use the commands `dir` and `gci`.

### `cd` (Change Directory)

- Example: `$ cd Documents`
- Lets you move around in directories.
  - The example here would move you into the Documents folder.
- `..` is an abbreviation for "one directory up".
- `.` means current directory.
- `cd` and `cd ~` move you back home.
- Mac/Linux: `/`, Windows `\`.
- If you have folders with spaces or weird characters, use "" marks. Example `cd "Program Files (x86)"`.
  - Non-weird characters are letters, numbers, `_`, `-`, and `.`.
  - Don't name folders with spaces or weird characters, please.

### `man` (MANual)

- Exmaple `$ man ls`.
  - Windows cmd/PS: `help` 
- Gives documentation for various commands.
- Arrow keys to scroll up and down, space bar to go "next page", `q` to quit.
- Some programs will have a `-h` or `--help` flag, which will do the same thing (or give a shorter bit of documentation).

### `mv` (MoVe)

- Example `$ mv ~/Documents/heb_contract.pdf .` 
- Lets you move files from one location to another.
  - Example moves the heb_contract.pdf file from Documents to the current working directory.
- `*` is a wildcard character - it will represent anything. `$ mv ~/*.pdf ~/Documents/pdfs/` will move every pdf in the home directory to the pdfs folder in the Documents folder.
- You can also use this to rename files.

### `cp` (CoPy)

- Example `$ cp ~/Documents/heb_contract.pdf .` 
- Like move, but copies a file instead.

### `rm` (ReMove)

- Example `$ rm old_data.txt`
- Removes a file.
  - This does *not* go into a Recycling Bin/Trash. The file is __GONE__ unless you want to start doing heavy forensics.
- You can remove folders only if they're empty.
  - `rm -rf` overrides this but you can delete __everything__ on your computer if you're not careful.

### `nano`
- Example: `$ nano code.py`
- Fairly simple text editor. Move around with arrow keys and type. 
- If you call this on a filename that doesn't exist, this lets you create a file.
- All the `^` commands at the bottom are Control commands (`^X` means Control-x).
  - You save (or discard changes) after giving the `^X` command (it'll ask for yes or no).

### `ssh` (Secure SHell)
- Example `$ ssh ONEPASSID@edgenode.heb.com`.
- Logs you in to another computer.
  - For our HEB computers, you need to have 2-factor authentication running.
- You shouldn't need to remember a long string for SSHing in. If you use your [.ssh config file](https://scotch.io/tutorials/how-to-create-an-ssh-shortcut), you save so much headache.
- If you quit `ssh` (call `exit`, close terminal, or lose internet/2FA) any programs running will die.
  - If this happens in the middle of a 2 hour Impala run, you're allowed to blast all the sad music you want at work.

### `scp` (Secure CoPy)
- Example `$ scp ONEPASSID@edgenode.heb.com:~/data.csv ~/`
- Like copy, but you can move files from one computer to another.
  - Example will move data.csv from the home directory of the edge node to the home directory of your computer.
- If you set up your .ssh config file, scp is a lot easier as well.

### PuTTY and FileZilla
- FileZilla can replace `scp`.
  - You gain a GUI, you lose some scripting quickness.
- PuTTY is a Windows program that handles `ssh` for you.
  - Gives you a terminal on the remote computer, but at least you can save your config with it.
  - Same issue of if PuTTY dies/you lose internet, it can kill your program.

### `screen`
- Screen is how you work around the SSH-dying problem.
- On your remote computer, run `$ screen` to start up a session. You can run your code, and if your connection dies your program won't.
- If you press Control+a+d while in a screen, you leave it.
- You can reconnect to your screen with `$ screen -r`.
- More instructions [here](https://www.linode.com/docs/networking/ssh/using-gnu-screen-to-manage-persistent-terminal-sessions/).


### `.bashrc` and `.bash_profile`

- Hidden files in home directory.
- It's a little confusing which one runs, but people just stick the following line of code in `.bash_profile` so bashrc always gets run.
```shell
if [ -f $HOME/.bashrc ]; then
        source $HOME/.bashrc
fi
```
- Mac does things slightly different than Linux, because making life difficult is fun. Just stick in the above code.
- Use `alias` commands here to give things nicknames so you never need to copy-paste something long and painful.
  - `alias something="something much longer"` lets you call "something much longer" with the shorter command "something".
  - Example: `alias run_impala="impala-shell -i IMPALAACCOUNT -k --ssl --ca_cert=/opt/cloudera/security/CAcerts/rootCA.pem -f"` means you can call  `$ run_impala impala_file.sql` to run your program.
- These scripts only get run when a terminal starts, so if you just edited the file and want to experience the changes, run `$ . .bashrc` (or `$ . .bash_profile`) after editing.
- Every user will have their own bashrc and bash_profile, so you need to create your own (and you don't need to worry about interfering with anyone else).

### Outputting results to a file
- Example `$ run_impala impala_file.sql &> impala.log`
- Using `&>` at the end of a shell command puts all the output to a file as opposed to printing to the terminal.
  - Result above creates a file called `impala.log` and puts everything in it. 
  - If you use `>` instead of `&>`, you'll have most of the stuff but may lose error messages.
- If you're running something that you're concerned may fail, better to have a file you can look at rather than trying to diagnose after the fact.
  - Screen won't save everything for you. 

## Challenge

I've put an Impala script for this session on Confluence. Download it, copy it to the edge node (on your user account), and run it with impala-shell.

If you enjoy the feeling of reducing future workload, copy the alias code (with the correct Impala account - I'll provide it on Slack) to the .bashrc file on the edge node and enable it.
