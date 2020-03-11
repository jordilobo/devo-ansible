# Docker Devo Ansible

## Visit [GitHub Devo-Ansible](https://github.com/DevoInc/devo-ansible) for full information: 

Docker Devo container tool: Designed to install Devo solution can be used to run ansible without to install it in your personal computer.

Tools inside:

    1. ansible 2.8.4 [python 2.7.15+]
    2. python 3.6.8
    3. nodejs-8.16.1
    4. curl
    5. coreutils
    6. inotify-tools
    7. vim
    8. less
    9. tree
    10. netcat-openbsd
    11. net-tools
    12. make
    13. tcpdump
    14. ngrep
    15. telnet
    16. openssh-server
    17. openssl
    18. perl
    19. dstat
    20. htop
    21. fio
    22. musl
    23. rsync
    24. rsyslog
    25. rsyslog-gnutls
    26. nmap
    27. sysfsutils
    28. pciutils
    29. iputils-ping
    30. python-netaddr

Project to deploy a Devo docker environment using docker swarm without to be necessary install anything.

This docker needs two directories to run and deploy Devo solution

1. $PWD/ssh_customer, directory bind to /root/.ssh-ori.
      * Bind to /root/.ssh-ori inside the container is used to make a copy and set the correct permissions and users to be used as ssh config for the container.
      * It will be used by ansible container to connect to the servers where you want install something with your playbooks
2. $PWD, current directory: the directory with all that you need to run inside the container, ansible playbooks, scripts, whaterver you need.
      * Bind to /opt/devo-ansible will be the directory with all your information that you want and need to share with the container

## Get devo-ansible:2.8.6 container

1. Download it before: run `docker pull devoinc/devo-ansible:2.8.6` (from Docker Hub Devo offical repository)
2. This docker container is ready to install Devo using Docker Swarm
3. This container has all necessary tools to install Devo. Follow the instructions to do it
4. Docker requirements should be done in server before to install Devo.
5. This container can be used to install whatever you need with ansible

## Run devo-ansible container:

1. From the directory where you have your playbooks, customer_hosts and ssh_customer directories run:
    1. `docker run --name devo-ansible -v $PWD/ssh_customer:/root/.ssh-ori -v $PWD:/opt/devo-ansible -d devoinc/devo-ansible:2.8.6`
2. Now you are ready to use ansible inside the container with all your playbooks and tools shared. All changes that you can do inside the container in /opt/devo-ansible will be saved outside in the host.

### Connect to the container:

1. docker exec -it devo-ansible /bin/bash
2. One time inside container run the next commands
    1. Go to: `cd /opt/devo-ansible`: It's the path where you bind your project directory when run docker.
    2. Reachable servers test: `time ansible all -i customer_hosts -m ping`
        * Check the results: all ping should answer pong
        * It means that you configured [ssh_customer](./ssh_customer) directory and [customer_hosts](./customer_hosts) file correctly
    3. Run your playbooks: `time ansible-playbook -i customer_hosts your_ansible_playbook.yml`

### Ansible helps:

1. `--ask-pass yourPassword` (ask pasword for ssh connection if we haven't pem file)
2. `--extra-vars "ansible_sudo_pass=yourPassword"` (to indicate sudo password if it's necessary in remote servers and your user can not run sudo without password)


### When all finish, run the next commands to stop and delete the devo-ansible container from memory

1. docker stop devo-ansible
2. docker rm devo-ansible

#### NOTES:

1. Look after that your pem file has the right permissions: 600
    * -rw------- 1 your_user your_user 1674 dic 15 19:10 your_file_name.pem
2. Remember: You can download the image using: `docker pull devoinc/devo-ansible:2.8.6`

### Enjoy it
