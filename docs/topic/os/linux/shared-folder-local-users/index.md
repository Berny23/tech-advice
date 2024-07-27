# How to Share a Folder Between Local Users in Linux

## Introduction

When searching for this problem, you will come across many people suggesting Samba to share folders. However, this is using a network protocol that allows other people to connect to your device. That's not exactly what I would call locally, although it should work there too.

**This guide shows how to create a shared folder without additional software and with basic GNU/Linux tools only.**

## Guide

This cannot easily be done in a GUI like Dolphin, so you have to use a shell.

1.  Create a new group for all users who should have access to the shared folder.

    ```shell
    sudo groupadd shared
    ```

2.  Add all desired users to the new group, for example user1 and user2.

    ```shell
    sudo usermod -a -G shared user1
    sudo usermod -a -G shared user2
    ```

3.  Create the shared folder in a location every user has read access to.

    ```shell
    sudo mkdir /home/Shared
    ```

4.  Assign the group to the new folder.

    ```shell
    sudo chgrp shared /home/Shared
    ```

5.  Grant read, write, execute permissions to the owner ("root") and the group ("shared"), all others get no access.

    The letter "s" in the group permissions sets the SetGID bit. This means that all new files and folders inside this directory inherit the parent's group ("shared") automatically.

    ```shell
    sudo chmod u=rwx,g=rwxs,o=- /home/Shared
    ```

6.  Add ACL permissions to the shared folder, essentially setting the default permissions for all new files and folders inside this directory.

    ```shell
    sudo setfacl -d -m u::rwx /home/Shared
    sudo setfacl -d -m g::rwx /home/Shared
    sudo setfacl -d -m o::- /home/Shared
    ```

## Notes

To allow access to other users later, just add them to the group like in step 2.

I hope this guide was helpful to you. Please consider giving the GitHub repository a star. :)

## Credits

* Guide by [Berny23](https://berny23.de)

* [ACL commands](https://unix.stackexchange.com/a/1315)
