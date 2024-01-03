# Useful File and Session Management Commands

## Description:

The commands provided are used for managing and monitoring files and sessions in a storage array environment. They include commands to:

- View all open files on the array and to search for a specific open file using a grep command.
- Forcibly close a file using its ID.
- View open sessions and to close sessions from a specific computer.
- Identify files that are being read, written, blocked, or contended, with options to limit and detail the output.

## Command to view all open files on the array. And grep to search for a certain file:

`isi_for_array isi smb openfiles list -v` : view all open files on the array

`isi_for_array isi smb openfiles list -v | grep -I <Filename>` : search for that open file

## Force close that file using its ID:

`isi smb openfiles close <id> [--force]`

## View open sessions:

`isi_for_array isi smb sessions list`

## Close those open sessions as well, From that computer:

`isi smb sessions delete <computer-name>`

## Which file are being contented, written and read:

`isi statistics heat --limit 30 --long --events read,write,blocked,contended`
