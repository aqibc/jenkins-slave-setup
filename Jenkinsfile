pipeline {
    agent {
      docker {
        image "ansible/ansible:default"
        label “slave-builder”
        args '-u 0:0 -v /home/jenkins/.ssh:/root/.ssh'
       } //docker
     } //agent
     stages {
         stage("Install Ansible") {
             steps {
          sh """
         pip install ansible
         """ 
             } //steps
         } //stage
         stage("Build") {
         steps {
             sh """
             ansible-playbook -i inventory jenkins-slave-setup.yml
             """
         } //steps
         } //stage
            stage("Test") {
             steps {
                 sh """
                 ansible jenkins-slaves -i inventory -m shell -a "ls /home/jenkins/jenkins_slaves"
                 ansible jenkins-slaves -i inventory -m shell -a "id jenkins"
                 ansible jenkins-slaves -i inventory -m shell -a "cat /home/jenkins/.ssh/authorized_keys"
                 """
             } //steps
         } //stage
        stage("Deploy") {
            steps {
                sh """
                    # one method
                    zip "jenkins-slave-setup.zip" .
                    scp jenkins-slave-setup.zip root@192.168.1.115:/var/www/html/my-repository
                    """
            } //steps
        } //stage
     } //stages
} //pipeline