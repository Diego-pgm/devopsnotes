### Package Management

- Centos uses an RPM (Red Hat Package Manager) to manage packages.

- A software is packaged into a bundle with the extension .rpm.

*Install packages*

rpm -i <package-name>.rpm


*Uninstall packages*

rpm -e telnet


*Query packages*

rpm -q telnet


- This will install only the software but not all the required dependencies for it to run.

- We need a single command that can query package, find its location and install all dependencies as well.

- YUM is a high level package manager that uses RPM underneath.

*Install packages*

yum install ansible


- yum finds where all the information about the repository in a configuration file in /etc/yum.repos.d

*YUM Repos*

- To see a list of yum repositories installed:


yum repolist


- The same files should be at /etc/yum.repos.d/

ls /etc/yum.repos.d


- It sometimes stores old versions, it is better to go to the software documentation and follow the instructions to download newer versions.

*List of installed or available packages*

yum list <package-name>


Example:

yum list ansible


*Remove packages*

yum remove <package-name>


*List all versions*

yum --showduplicates list ansible


*Install specific version*

yum install ansible-2.4.2.0


### Services

- Once software is installed on servers, especially those that run in the background, such as web servers or database servers, we need to make sure that those services are running and they are still running after the server is rebooted.

- Services in Linux help to configure software to run in the background and make sure that they run all the time automatically when servers are rebooted as well as they follow the right order of startup.

*Start Services*

service httpd start



systemctl start httpd

*Stop Service*

systemctl stop httpd


*Check status*

systemctl status httpd


*Configure to start at startup*

systemctl enable httpd


*Configure HTTPD to not start at startup*

systemctl disable httpd


- A systemd service are the services running in the background and they are configured using a systemd unit file.

- The location of this files are under the path and they ares stored as: /lib/systemd/system/service-name.servce

*Example python flask app as systemd service*

[Unit]
Description=My python web application

[Service]
ExecStart=/usr/bin/python3 /opt/code/my_app.py
ExecStartPre=/opt/code/configure_db.sh
ExecStartPost=/opt/code/email_status.sh
Restart=always

[Install]
WantedBy=multi-user.target
