<img src="file:///C:/Users/stenh/AppData/Roaming/marktext/images/2024-10-16-15-28-07-image.png" title="" alt="" width="186">                                                          Sten Hulsbergen - 20242689

# Session 4

## 4.1

To find files that are not owned by root, use: `find /var/spool -not -user root -ls`, which means: find *(find)* files in a directory *(/var/spool)* that are not *(-not)* from root *(-user root)* and list them *(-ls)*.

As shown with `ls -l` the directory only contains 1 folder 'mail', with root as owner, which will not be displayed as seen in the output of the first command.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-14-31-image.png)

## 4.2

Become root with `sudo su`, make a file in mnt-directory with `touch /mnt/test.txt`, followed by `exit` to go to normal user and `ls -l /mnt/test.txt` to see who owns the file.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-18-10-image.png)

## 4.3

Adding a user with `sudo adduser user1` and giving it password 'user1'.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-22-02-image.png)

## 4.4

Becoming root as user1 will not work at first because it has no sudo permissions as shown bellow.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-24-52-image.png)

Change back to other user and edit `nano /etc/sudoers` and add user1 with all privileges like shown bellow. 

(personally would have used `usermod -aG sudo user1` for this, but the exercise says specifically to 'edit the /etc/sudoers')

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-29-39-image.png)

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-29-19-image.png)

Go back to user1, this time `sudo su -` will work.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-30-32-image.png)

## 4.5

Creating a file as root while being user1 with `sudo touch /mnt/test2.txt` and use `ls -l /mnt/` to see who owns the file.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-34-12-image.png)

## 4.6

Before installing packages, make sure everything else is up-to-date with `sudo apt update && sudo apt upgrade`. 

(The reason why i use `apt` instead of `apt-get` is because it is the newer version)

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-42-48-image.png)

Now other packages can be installed without worrying about older versions of certain dependencies.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-23-14-37-30-image.png)
