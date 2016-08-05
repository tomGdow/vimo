## vimo

vimo is a bash script that opens a file in vim from anywhere within an   
application such as that created by Ruby-on-Rails, without having   
to change directory or specify a filepath.

### Demonstration
  
A simple demonstration may be found [here](https://asciinema.org/a/as606114p3ph827jeoue7h432)

### Download 
On an Ubuntu/Xubuntu OS:


     (1) Check if your home directory already contains a 'bin' directory. 
         ls  ${HOME} | grep  bin 
         If this directory does not exist, you will need to create it.
         mkdir ${HOME}/bin
         Add the new directory to your path (or, alternatively, logout and 
         login again)
         PATH=${PATH}:${HOME}/bin
         On most Ubuntu/Xubuntu installations, ${HOME}/bin  will already
         be present.

     (2) Clone the repository.
         git clone https://github.com/tomGdow/vimo.git
         You should end up with the following directory struture.
         .
         └── vimo
             ├── README.md
             └── vimo

     (3) From within the directory 'vimo', move the file 'vimo' to ${HOME}/bin. 
         cd vimo
         mv vimo "${HOME}/bin"

     (4) It may be necessary to change permissions.
         chmod u+x "${HOME}/bin/vimo"

     (5) As an alternative to (3) & (4), create a symlink to 'vimo' in '${HOME}/bin'.
         cd ${HOME}/bin
         ln -s /path/to/vimo/vimo vimo

     (6) It may be necessary to change permissions.
         cd vimo
         chmod u+x "vimo"

     (7) Try it out.
         vimo -h

### Description    

    NAME
           vimo
    
           This script opens a file in vim from anywhere within an application
           framework.  
    
    SYNOPSIS
            vimo [-c] [-d] [-f] [-h] [-i] [-l] [-m] [-v] [-x] <search term> [filename1 
            filname2 ...]
    
    DESCRIPTION
    
           vimo (vim-open) may be used to open any file from within an application 
           root directory without the need to change directory or specify 
           file paths. For example, 'vimo routes.rb' opens the 'routes.rb' 
           file from any directory within a Ruby-on-Rails application.  
    
           The script is written with Ruby-on-Rails and Middleman in mind. 
           It works best from within applications generated by these frameworks,
           but it may be used anywhere (albeit with restricted functionality).
    
           It is also not restricted to vim, but will open files in any editor
           specified by the EDITOR ENV variable
    
           Directories may be searched for, but not opened in vim, 
           using the '-d' option.
    
           Outside the root directory, the file path may have to be specified.
    
           If more than one file or directory is found, the output is a 
           numbered list, from which a file (but not a directory) may be 
           selected and opened in vim. 
    
           If only a single file is found, it is by default opened 
           automatically in vim.
    
           If more than one non-optional argument is given, all files (not
           directories)  are opened in vim provided they exist in the
           current directory. This default behaviour may be over-ridden using
           the '-m' option. 
           
           To obtain the output of a search in a form suitable for piping
           to another Unix/Ubuntu command, use the '-x' option (unix-mode),
           which also over-rides the ability to open the output in vim
           
           To search for multiple terms, use the '-m' option. 
    
           The basic search command is of the following form, 
           where VIMODIR strives to be the application root directory.
    
              find VIMODIR -type f -name <search-term> 
    
    OPTIONS 
    
          -c: use current directory. When this option is used, the basic 
              search command is of the type:
             
              find . -type f -name <search-term>
           
          -f: give the full directory path
          -d: search for directories only
          -h: get help
          -i: case-insensitive search
          -l: give a list of file locations only (open in text editor 
              functionality disabled) 
          -m: search for multiple terms 
          -v: verbose mode. Gives, among other things, a list of functions
          -x: unix mode. Gives search output in a form suitable for piping to
              another Unix/Ubuntu command 
    
    REQUIREMENTS
          
          (1)  The EDITOR ENV variable need not be set, but may be set to an
               editor of choice. The default value is vim.
               You may wish to execute the following prior to running the
               script. 
               EDITOR='vim'
               export EDITOR
               Or, include the following line in your .bashrc file
               export EDITOR='vim'
    
          (2)  The VIMODIR ENV variable controls the working directory and
               strives to be the application root directory.
               It may be changed to the current working directory as follows:
               VIMODIR=$(pwd) 
               export VIMODIR
               Alternatively,
               VIMODIR='/full/path/to/working/directory' 
               export VIMODIR
               Or, include the following line in your .bashrc file
               export VIMODIR='/full/path/to/working/directory'
         
          (3)  The VIMOMAX ENV variable controls the maximum number of 
               search results shown. If the number exceeds VIMOMAX, the user
               is given the choice whether to continue or abort.
               This ENV variable need not be set, and defaults to the value
               of 30. It may be changed as follows:
               VIMOMAX=50
               export VIMOMAX
               Or, include the following line in your .bashrc file
               export VIMOMAX=50
    
          (4)  The VIMOCHOICE ENV variable controls how multiple files are 
               opened in vim. If more than VIMOCHOICE files are to be 
               opened (where, for example,the command contains multiple 
               search terms without the '-m' option), the user will be 
               given a choice whether to proceed or abort.
               This ENV variable need not be set, and defaults to the value
               of 2. It may be changed as follows:
               VIMOCHOICE=3
               export VIMOCHOICE
               Or, include the following line in your .bashrc file
               export VIMOCHOICE=3
    
    NOTES
          (1)  Tested with Rails 4 and Rails 5, and Middleman with Ubuntu 14
               and xubuntu 16.  
          (2)  It is best to double-quote search terms, especially if 
               wildcards are included. 
          (3)  vimo may be used to search for either files or 
               directories, but not both.  The default behaviour is to 
               find files only.
    
    EXAMPLE USAGE
          (1)  vimo "routes.rb"  
          (2)  vimo -l "index*"   # list only 
          (3)  vimo -cl "index.html.erb" # list only from current directory
          (4)  vimo -clf "index.html*"   # list only from current directory
               and give full paths
          (5)  vimo -i "gemfile"  # case-insensitive
          (6)  vimo -vil "gemfile"  # verbose mode, case-insensitive, list only
          (7)  vimo -d  "stylesheet*" # directory search
          (8)  vimo "fileOne" "fileTwo"  # Open both files in vim provided that
               they exist in current directory
          (9)  vimo -m "fileOne" "fileTwo" # Multiple search terms
          (10) vimo -md "dirOne" "dirTwo"  # Multiple serch terms, directories only
          (11) vimo -dix "stylesheets"  # Unix-mode, case-insensitive, directories only
    
    ADVANCED USAGE
          (1)  vimo -dx "db" | xargs tree
          (2)  vimo -dx "assets" | sed -n "1p" | xargs tree -L 1
          (2)  vimo -x "index*" | sed -n "2p"| xclip
    
    AUTHOR
          tomgdow (thomasgdowling@gmail.com)
          
