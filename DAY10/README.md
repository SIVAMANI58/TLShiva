File Management Script

This script allows users to create, delete, or move files through a simple command-line menu. It uses Bash case statements and basic file-handling commands.
Features
###1. Create a File
Prompts the user to enter a file name.
Checks if the file already exists.
If it does not exist, it creates the file with a .txt extension.

###2. Delete a File
Asks for a file name to delete.
Deletes the file if it exists; otherwise notifies the user.

###3. Move a File
Prompts for a file name and destination directory.
Moves the file if both file and directory exist.

Simple Bash Menu Script

This Bash script provides a basic menu that allows the user to create a file, create a directory, or list files in the current directory.
Features
###Option 1 — Create a File
Creates a file named siva.txt using the touch command.

###Option 2 — Create a Directory
Creates a directory named siva using the mkdir command.

###Option 3 — List Files
Displays the list of files and directories in the current location using the ls command.

##Invalid Option
If the user enters any other value, the script displays:User Input Validation Script
This Bash script collects a user's name and age, then performs validation checks to ensure both values are entered correctly.
Features
### Name Input
Checks if the name field is empty.
Ensures the name contains only letters and spaces using a regular expression.

### Age Input
Checks if the age is empty.
Ensures the age is a number between 1 and 99.


