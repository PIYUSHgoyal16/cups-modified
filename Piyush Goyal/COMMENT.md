PRINTING COMMENTS IN LOG FILE - 2020-06-02
==========================================

This file describes the approach of adding comments in CUPS files and printing 
them in default log files. It covers details from the start and no prerequiste 
is required to understand the same.


INSTALLING CUPS
---------------

The first step before installation is to remove the preinstalled system CUPS 
from the machine. This can hinder in testing of our modified code. Removal of
system CUPS can be achieved through following command:

    sudo apt-get remove cups

In my case some of the CUPS folders were not removed and hence I manually removed 
them. 

The next step is to clone the cups repository through the following command:

    git clone https://github.com/apple/cups.git

Once the project was cloned, I moved into the cups directory and configured 
CUPS for my system through the usual "configure" script in the main CUPS source
directory, using the following command:

    ./configure

After configuration, wrote the make command. Tested it and installed the same using:

    make 
    make check
    sudo make install

Start the CUPS service using:

    sudo /usr/sbin/cupsd -f


ADDING PRINTERS & CREATING PRINT QUEUES 
---------------------------------------

I created printers in two ways. First was using lpadmin command. For other I used
ippserver command to  generate my print server.

    /usr/sbin/lpadmin -p nowhere -E -v file:/dev/null
    /usr/sbin/ippserver everywhere

Next to create print jobs I used the lp command.

    lp -d nowhere <file_name>

Kindly note that for installation I have used the default configuration and each and 
every command is executed in the home directory. The location of these commands may 
vary according to the configuration used at the time of installation. 


FINDING LOG FILES AND RELEVANT CODE
-----------------------------------

I found the default location of log files in cups/conf/cups-files.conf.in. The log files 
were stored in /var/log/cups. There were two log files, access_log and error_log. I noted
some of the keywords in access_log and used grep command to find relevant files that could
generate them.

    grep -r "localhost - -" cups/

The result displayed many files. The most relevant file found was log.c located in scheduler
folder. I went through the file and found a function cupsFilePrintf().

The function accepted two arguments, i.e. the file name and the text to be written in that
file.


ADDING COMMENTS AND PRINTING THEM
---------------------------------

I added some comments in the cupsFilePrintf() function. I added my name in the function. 
After changing the file. Enter make and make install commands again. 

    make
    make install

Stop the CUPS service using Ctrl+C and start again using the following command.

    sudo /usr/sbin/cupsd -f

Again give any print job and check the log files. The added comments will be printed in 
the log files.

Note that the cupsFilePrintf() function is present two times. One for accesss_log and other 
for error_log. Check for the changes in whose cupsFilePrintf() you have added the comments.