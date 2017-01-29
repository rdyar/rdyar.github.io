---
---

Just some random notes from a project - the Simple Ordering System.

## Windows Command Line Stuff

Command prompt - not PowerShell to see a list of Environment Variables: type `set`. Only works in the command prompt window, not PS.

Powershell: set an environment variable: `$env:API_URL = "http://localhost:3100/api"`
To read it back: `echo $env:API_URL`

## Ubuntu Command Line Stuff

Updates available - install them: `sudo apt-get update && sudo apt-get upgrade` 
the upgrade note called a message of the day, may not go away until the next day or reboot.

Check disk information - to see free space etc: df -h  
human readable ls with last modified date: ls -halt  
show current date and time: date "+%H:%M:%S   %d/%m/%y"

top - shows list of processes
shift + m sort top by mem usage

### Ubuntu and Vi

Save and quit - :wq - make sure insert is off - press escape  
Just quit - :q  
paste - right click  

To remove a block of text:

- Put your cursor on the top line of the block of text/code to remove.
- Press V (That's capital "V" : Shift + v )
- Move your cursor down to the bottom of the block of text/code to remove.
- Press d.

### PM2
logs are in home/.pm2 

## Javascript Debugging

`window.alert((JSON.stringify(fileNames)) + ',' + dupeCheck + ',' + i);`

