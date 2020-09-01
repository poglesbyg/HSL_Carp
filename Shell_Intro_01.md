# First Steps with the Shell

## What is the Shell and Why Do I Care?

* A shell interprets commands you type, it does many of the things you'd normall do with a mouse using a graphical user interface (GUI), such as moving and copying files.
* Learning to use the shell requires more up front effort, but in the long term can save you time
  * Most of the commands are cryptically short, but are often abbreviations of some sort
* Many commonly used bioinformatics programs and tools are written on Unix style systems
  * You'll get the most power out of these tools when you are access them in their native environment
  * Web and GUI interfaces exist for a number of tools, but rarely offer the full range of options available to them
* The shell is how you access powerful compute clusters like UNC's *Longleaf* system.
  * The cluster hardware offers more compute power and memory than your local machine
  * You can use the shell to automate some tasks
  * Its through the shell you can submit large computation tasks to the system's queue


## Login

**demo ssh program**

If you have a local terminal program, such as Terminal on MacOS X, you can connect to longleaf with this command:

`ssh <onyen>@longleaf.unc.edu`
  
Where you should replace `<onyen>` with your own onyen. Since my onyen is `tristand`, for me the command is:  
  
`ssh tristand@longleaf.unc.edu`

You'll be prompted for your onyen password.



## Finding Yourself and Going Places

The `$` indicates the prompt, telling you the shell is ready to accept a command.  By default, the prompt on Longleaf will look like this:

> ~~~
> [tristand@longleaf-login2 ~]$
> ~~~

Except with your own onyen instead of `tristand`, and you may be on either `login1` or `login2` - these are just two nodes you are arbitrarily assigned when logging in.  In the lesson examples below, we'll just shorten it to `$`, what comes after it is what you'll type.

Let's use our first command, `pwd`, which stands for *print working directory*

~~~
$ pwd
~~~

Which results in:

> ~~~
> /nas/longleaf/home/tristand
> ~~~

This is the location of your home directory, where you will always be when logging in.  Each word separated by a slash is a directory within the directory to its left.  Directories are essentially the same as folders in Windows or MacOS.  So `tristand` is a subdirectory of `home`, which is a subdirectory of `longleaf`, etc.  The first `/` is a special directory, the root directory of the filesystem.

However, there isn't much disk space allocated to individual home directories.  For genomics data sets, you'll want to work in the scratch space or your group's `/proj/<labname>/` directory - we'll talk about the `/proj` space in a future lesson.  All PI's can request a directory in /proj from ITS research computing.





We can see files and subdirectories are in this directory by running `ls`, which stands for *listing*:

~~~
$ ls
~~~

There shouldn't be anything here yet, unless you've used your scratch space before, like me.  We're going to use the `cp` command, which is short for *copy*.

`cp` takes two main arguments, the first is where you're copying *from*, and the second where you're copying *to*.  There's also the `-r` in there, this is a special argument referred to as an *comand-line option* , we'll cover these later in the lesson.

~~~
$ cp -r /proj/seq/data/carpentry/shell_data/ .
~~~

In this case, `/proj/seq/data/carpentry/shell_data/` is where we're copying *from*, and `.` the *to*.  '.' is a special shell notation meaning essentially *here*, whichever directory you are currently in.  Once the prompt returns, ie the copying has finished, you can use `ls` again to see the directory you've copied, `shell_data`

The Unix shell uses spaces and whitespace as separators, so we typically use an underscore in naming files with multiple words.

Let's enter the newly copied directory

~~~
$ cd shell_data
~~~

and then do a listing on it

~~~
$ ls
~~~

which should produce:

> ~~~
> sra_metadata  untrimmed_fastq
> ~~~

We've just used a few commands.  While `pwd` just works on its own, `cd` required an argument - the destination you wanted to go to.  Most of the basic shell commands typically use 0 to 2 arguments.  You can think of `cd` command in the following way:

~~~
$ cd <location you want to go to>
~~~

In these lessons, code with text between `<some text>` means to fill in that text with an appropriate argument.

`ls` prints the names of the files and directories in the current directory in alphabetical order, arranged neatly into columns.

We can make `ls` output more information by using the option `-F`, also called a *flag*, which tells `ls` to add a trailing `/` to the names of directories:

~~~
$ ls -F
~~~

> ~~~
> sra_metadata/  untrimmed_fastq/
> ~~~


Anything with a "/" after it is a directory. Things with a "\*" after them are programs. If there are no decorations, it's a file.

A lot of modern terminal applications color different file types, so `-F` isn't as essential as it once was, but it can still be useful.

The options to Unix commands are specified with a `-`, or `--` for more complex options, which makes them slightly different than the regular arguments to a command, such as the location you want to go with the `cd` command.

`ls` has lots of other options. To find out what they are, we can use the `man` command, which was shortened from *manual*.

~~~
$ man ls
~~~

Some manual files are very long. You can scroll through the file using
your keyboard's up and down arrows or use the <kbd>Space</kbd> key to go forward one page
and the <kbd>b</kbd> key to go backwards one page. When you are done reading, hit <kbd>q</kbd>
to quit.

> ## Challenge
> Use the `-l` option for the `ls` command to display more information for each item 
> in the directory. What is one piece of additional information this long format
> gives you that you don't see with the bare `ls` command?

***  

***  

Do this on your own before looking below - think about the information displayed for a moment.  

***  

***  

> > ## Solution
> > ~~~
> > $ ls -l
> > ~~~
> > 
> > > ~~~
> > > total 0
> > > drwxr-xr-x 2 tristand its_employee_psx 4096 Feb  6 20:33 sra_metadata
> > > drwxr-xr-x 2 tristand its_employee_psx 4096 Feb  6 20:33 untrimmed_fastq
> > > ~~~
> > 
> > The additional information given includes the name of the owner of the file,
> > when the file was last modified, and whether the current user has permission
> > to read and write to the file.  We'll cover permissions in a later lesson.
> > 

No one can possibly learn all of these arguments, that's what the manual page is for. You can (and should) refer to the manual page or other help files as needed.

Let's go into the `untrimmed_fastq` directory and see what is in there.

~~~
$ cd untrimmed_fastq
$ ls -F
~~~

> ~~~
> Control_A.fastq  Control_SR.fastq  SRR097977.fastq
> Control_B.fastq  file_sizes.txt    SRR098026.fastq
> ~~~

This directory contains a few files with `.fastq` extensions. FASTQ is a format for storing information about sequencing reads and their quality. We will be learning more about FASTQ files in a later lesson.

The `-F` option isn't too informative, but there are other `ls` options which are useful.  By default, `ls` tries to group files is as many columns as the terminal window permits.  You can use the `-1` (the number, not lowercase L) to force the files listed in a single column.

~~~
$ ls -1
~~~

> ~~~
> Control_A.fastq
> Control_B.fastq
> Control_SR.fastq
> file_sizes.txt
> SRR097977.fastq
> SRR098026.fastq
> ~~~

This can be useful when you need to construct a list of files.


Where are we currently?  Never be afraid to use `pwd` a lot to remember where in the file system you are - once you learn more powerful commands, you want to make sure you're using them while in the right directory.

~~~
$ pwd
~~~

> ~~~
> /pine/scr/t/r/tristand/shell_data/untrimmed_fastq
> ~~~

So far, we've always been heading down into the nesting of directories, we can jump up one directory:

~~~
$ cd ..
~~~

`..` is another special shell notation, indicating the directory on level above your current directory.

~~~
$ pwd
~~~

> ~~~
> /pine/scr/t/r/tristand/shell_data
> ~~~

Now, move up one more level on your own, and confirm your location

## Shortcut: Tab Completion

Typing out file or directory names can waste a lot of time and it's easy to make typing mistakes. Instead we can use tab complete as a shortcut. When you start typing out the name of a directory or file, then hit the <kbd>Tab</kbd> key, the shell will try to fill in the rest of the directory or file name.

In the example below, you're typing 'cd she' and then hitting the <kbd>tab</kbd> key once.

~~~
$ cd she<tab>
~~~


The shell will fill in the rest of the directory name for `shell_data`.  Once it autocompletes, you can hit enter to execute

Now try to change directories to `untrimmed_fastq` in `shell_data` using autocomplete.

~~~
$ cd untrimmed_fastq
~~~


Using tab complete can be very helpful. However, it will only autocomplete a file or directory name if you've typed enough characters to provide a unique identifier for the file or directory you are trying to access.

What happens when we try the following (only hit <kbd>tab</kbd> once)

~~~
$ ls SR<tab>
~~~

The shell auto-completes your command to `SRR09`, because all file names in the directory begin with this prefix. When you hit
<kbd>Tab</kbd> again, the shell will list the possible choices.

~~~
$ ls SRR09<tab><tab>
~~~

> ~~~
> SRR097977.fastq  SRR098026.fastq
> ~~~

Now let's try the following:

~~~
$ ls sr<tab><tab>
~~~

Nothing is happening, no matter how many times we hit <kbd>Tab</kbd>.  This is because the shell is case sensitive.  The following are different identifiers to the shell:

> ~~~
> SRR097977.fastq
> srr097977.fastq
> SRR097977.FASTQ
> ~~~


Tab completion can also fill in the names of programs, which can be useful if you remember the begining of a program name. 

~~~
$ pw<tab><tab>
~~~


~~~
pwd         pwd_mkdb    pwhich      pwhich5.16  pwhich5.18  pwpolicy
~~~


Displays the name of every program that starts with `pw`. 
