# vansible

A self contained build pipeline environment complete with static code analysis.

Separate vagrant configuration to provision the required servers on a locally running VirtualBox, with separate ansible configuration to install the required applications on the servers - jenkins, sonar, git, maven, etc.


## A few useful commands...

*  export ANSIBLE_HOST_KEY_CHECKING=False
*  ansible all -m ping -i inventory --ask-pass --ask-sudo-pass -u centos
*  ansible all -a "/bin/echo hello" -i inventory --ask-pass --ask-sudo-pass -u centos
*  ansible-playbook site.yml -i inventory --ask-pass --ask-sudo-pass -u centos -v
* java -jar "/var/cache/jenkins/war/WEB-INF/jenkins-cli.jar" -s "http://localhost:9000/jenkins" get-job Andy_Build1
