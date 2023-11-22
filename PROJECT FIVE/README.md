## SHELL SCRIPTING AND USER INPUT

### Introduction:

Shell scripting is an important part of process automation in Linux. Scripting helps you write a sequence of commands in a file and then execute them.

This saves you time because you don't have to write certain commands again and again. You can perform daily tasks efficiently and even schedule them for automatic execution.

### What is a Bash Script?
A bash script is a series of commands written in a file. These are read and executed by the bash program. The program executes line by line.

You can do the same sequence of steps by saving the commands in a bash script and running it. You can run the script any number of times. By naming conventions, bash scripts end with a .sh. However, bash scripts can run perfectly fine without the sh extension.


## Shell Script
#### Step 1.    
Open a folder called ***shell-scripting*** on your terminal, using the command `mkdir shell-scripting`. This will be our folder for theis project.

#### Step 2.    
Create a file in the folder called 
>       `touch user-input.sh`

![Alt text](<Images/First shell 1-2.png>)

#### Step 3.    
Copy and paste the code below into the file.
>       #!/bin/bash
>
>       #Prompt the user for their name
>
>       echo "Enter your name:"
>       read name
>
>       #Display a greeting with the entered name
>
>        echo "Hello, $name! Nice to meet you."

#### Step 4. 
Save your file.

#### Step 5.
Run this command to make the file executable > >        `chmod x user-input.sh`
#### Step 6.
Run the script using the command `./user-input.sh`
and you will get the output beneath.

![Alt text](<Images/First shell 3-4.png>)


## Directory Manipulation and Navigation
This script is intended to display the current directory,create a new directory called **my_directory**, change to that directory,create two files inside it, list the files, move back one level up, remove the **my_directory** and its contents, and finally list the files in the current directory again.

#### Step 1.
Open a file named ***navigating-linux-filesystem.sh***
>       touch navigating-linux-filesystem.sh
![Alt text](<Images/D MANIP 1.png>)

#### Step 2.
Copy the code below and paste inside the file

> #!/bin/bash
>
> #Display current directory
>
> echo "Current directory: $PWD"
>
> #Create a new directory
>
> echo "Creating a new directory..."
> mkdir my_directory
> echo "New directory created."
>
> #Change to the new directory
>
>
> echo "Changing to the new directory..."
> cd my_directory
> echo "Current directory: $PWD"
>
> #Create some files
>
> echo "Creating files..."
> touch file1.txt
> touch file2.txt
> echo "Files created."
>
> #List the files in the current directory
>
> echo "Files in the current directory:"
> ls
>
> #Move one level up
>
> echo "Moving one level up..."
> cd ..
> echo "Current directory: $PWD"
> 
> #Remove the new directory and its contents
>
> echo "Removing the new directory..."
> rm -rf my_directory
> echo "Directory removed."
>
> #List the files in the current directory again
>
> echo "Files in the current directory:"
> ls

![Alt text](<Images/D MANIP 3.png>)

#### Step 3.
Run the command 
>       `chmod +x navigating-linux-filesysytem.sh`

 This will execute permission on the file.

 ![Alt text](<Images/D MANIP 2.png>)  



 ## File Operations and Sorting
 1. This script will create 3 files 
 - file1.txt
 - file2.txt
 - file3.txt

 2. It will display the files in their current orders, 
 3. Sorts them alphabetically,
 4. Saves the sorted files in ***sorted_files.txt***, 
 5. Displays the sorted files, 
 6. Renames the sorted file to ***sorted_files_sorted_alphabetically.txt***, 
 7. And finally displays the contents of the final sorted file.


#### Step 1.
Create a file in your terminal called ***sorting.sh*** using 
>     `touch sorting.sh`

![Alt text](<Images/FILE OP 1.png>)

#### Step 2.
Copy and paste the code block below into the file

    #!/bin/bash

        #Create three files

    echo "Creating files..."
    echo "This is file3." > file3.txt
    echo "This is file1." > file1.txt
    echo "This is file2." > file2.txt
    echo "Files created."
        #Display the files in their current order
    echo "Files in their current order:"
    ls
        #Sort the files alphabetically
    echo "Sorting files alphabetically..."
    ls | sort > sorted_files.txt
    echo "Files sorted."

        #Display the sorted files
    echo "Sorted files:"
    cat sorted_files.txt

        #Remove the original files
    echo "Removing original files..."
    rm file1.txt file2.txt file3.txt
    echo "Original files removed."

        #Rename the sorted file to a more descriptive name
    echo "Renaming sorted file..."
    mv sorted_files.txt sorted_files_sorted_alphabetically.txt
    echo "File renamed."

        #Display the final sorted file
    echo "Final sorted file:"
    cat sorted_files_sorted_alphabetically.txt

![Alt text](<Images/FILE OP 2.png>)

#### Steps 3 & 4.

>       `chmod +x sorting.sh` 

This command will set execute permission.

Run your script using this command 
>   `./sorting.sh`

![Alt text](<Images/FILE OP 3&4.png>)





## Working with Numbers and Calculations

1. This script defines 2 variables 
- num1
- num2
with numeric values,
2. Performs basic arithmetic operations
- addition
- subtraction
- multiplication
- division
- modulus
3. Displays the result
4. Performs complex calculations such as raising num1 to the power of 2 and calculating the square root of num2
5. Displays the result



#### Step 1 & 2

Create a file on the terminal  called ***calculations.sh*** with this command `touch calculations.sh` Then go ahead and copy the code block below and paste in the file.

     #!/bin/bash

    # Define two variables with numeric values
    num1=10
    num2=5

    # Perform basic arithmetic operations
    sum=$((num1 + num2))
    difference=$((num1 - num2))
    product=$((num1 * num2))
    quotient=$((num1 / num2))
    remainder=$((num1 % num2))

    # Display the results
    echo "Number 1: $num1"
    echo "Number 2: $num2"
    echo "Sum: $sum"
    echo "Difference: $difference"
    echo "Product: $product"
    echo "Quotient: $quotient"
    echo "Remainder: $remainder"

    # Perform some more complex calculations
    power_of_2=$((num1 ** 2))
    square_root=$(echo "sqrt($num2)" | bc)

    # Display the results
    echo "Number 1 raised to the power of 2: $power_of_2"
    echo "Square root of number 2: $square_root"

![Alt text](<Images/NUMB 1&2.png>)

#### Step 3

Set execute permission on ***calculations.sh*** using this command 

>       `chmod +x calculations.sh`

![Alt text](<Images/NUMB 3.png>)


#### Step 4
Run the script with 

>       `./calculations.sh`

![Alt text](<Images/step 4.png>)





## File Backup and Timestamping

This scripting is very important, because it is focused on file backup and timestamp, which is a most common task for a DevOps Engineer.

The scripts 
- Defines the source directory and backup directory paths
- It the creates a timestamp using the current date and time
- it creates a backup directory with the timestamp appended to its name
- The script then copies all files from the source directory to the backup directory using ***cp*** command with the ***-r*** option for recursive copying.
- Finally, it displays a message indicating the completion of the backup process and shows the path of the backup directory with the timestamp.


#### Step 1.
On the terminal create a file ***backup.sh*** with this command 
>       `touch backup.sh`

![Alt text](<Images/BACKUP STEP 1.png>)

#### Step 2.
Copy and paste the code block below into the file

        #!/bin/bash

    #Define the source directory and backup directory
    source_dir="/path/to/source_directory"
    backup_dir="/path/to/backup_directory"

    #Create a timestamp with the current date and time
    timestamp=$(date +"%Y%m%d%H%M%S")

    #Create a backup directory with the timestamp
    backup_dir_with_timestamp="$backup_dir/backup_$timestamp"

    #Create the backup directory
    mkdir -p "$backup_dir_with_timestamp"

    #Copy all files from the source directory to the backup directory
    cp -r "$source_dir"/* "$backup_dir_with_timestamp"

    #Display a message indicating the backup process is complete
    echo "Backup completed. Files copied to: $backup_dir_with_timestamp"


![Alt text](<Images/BACKUP step 2.png>)

#### Step 3.
Set execute permission on the file using

>        `chmod +x backup.sh`

![Alt text](<Images/BACKUP chmod.png>)

#### Step 4.

Run your script using 

>       `./backup.sh`

![Alt text](<Images/BACKUP step 4.png>)


### Thank you!