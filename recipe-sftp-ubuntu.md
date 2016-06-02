Making a sftp on ec2 ubuntu machine
=
This recipe is created in order to describe the process for create an sftp jailed on an ec2 machine. 
> **Note:**

> - This recipe was tested on an **Ubuntu** machine
> - The machine is in natural status.

------

Firstly you must access like root or super user
>sudo su

We update the system, and instal openSSH
>apt-get update
>apt-get install openssh-server openssh-client

We create the new directories, users and folders for users
>mkdir /home/sftpserver
>mkdir /home/sftpserver/{USER}
>mkdir /home/sftpserver/{USER}/{FOLDER1}
>mkdir /home/sftpserver/{USER}/{FOLDER2}

We add a new user group and add a new user for that  group
>groupadd sftpserver
>useradd -g sftpserver -s /bin/false -d /home/sftpserver/lsizaguirre >lsizaguirre
>passwd lsizaguirre
>chown lsizaguirre:sftpserver /home/sftpserver/lsizaguirre/folder1
>chown lsizaguirre:sftpserver /home/sftpserver/lsizaguirre/folder2
>chown root:root /home/sftpserver
>chown root:root /home/sftpserver/lsizaguirre
>chmod 700 /home/sftpserver/lsizaguirre/folder1 //solo por usuario
>chmod 755 /home/sftpserver/lsizaguirre/folder2 //todos ven
>chmod 755 /home/sftpserver/lsizaguirre
>chmod 755 /home/sftpserver

We do a backup for configuration file an edit 
>cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
nano /etc/ssh/sshd_config

>cambiar a yes
PasswordAuthentication yes
comentar
Subsystem sftp /usr/lib/openssh/sftp-server
agregar al final
Subsystem sftp internal-sftp
Match user lsizaguirre
ChrootDirectory /home/sftpserver/lsizaguirre
ForceCommand internal-sftp

Restart the service
>service ssh restart