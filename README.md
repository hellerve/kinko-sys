# Some tools for the kinko base system

## Account management

### mkgroups

    mkgroups group ..

A shell script which creates one or more groups. 

### mkuser

    mkuser user [ group .. ]

A shell script which creates one or more groups. 

### lsgroups

    lsgroups [ pattern ]
    
A shell script which lists all groups, potentially filtered by a pattern.

**Example:**

    lsgroups kinko-test-*

### lsusers

    lsusers [ pattern ]

A shell script which lists all users, potentially filtered by a pattern.

**Example:**

    lsusers kinko-test-*

## Account switching

    run-as [ --pidfile path ] command ...

The run-as binary is to be installed under a specific name, which must match
the name of a user, and must be installed with 5710 access rights (`-rws--x---`)
and owned by `root`, for example using install like this:

    sudo install -o root -g kinko-test-server -m 5710 bin/run-as bin/kinko-test-server

The resulting command allows all members of the group *kinko-test-server* to run any
command as the *kinko-test-server* user.

# run-as examples

Consider the following scenario:

## interfacing via scripts

There is one application, which is managed by a `kinko-server` user. This application
provides two public interfaces via scripts: `read-server` and `write-server`. 
The `read-server` script reads the content of the server status, while the `write-server`
script writes the server status.

Only some users should be allowed to use `read-server`, and a different set of users
should be allowed to use `write-server`.

This scenario can be managed using the following implementation:

1. create a script `kinko-read-server` with this content:

        #!/bin/sh
        exec kinko-server read-server
    
    and install it under the name 
