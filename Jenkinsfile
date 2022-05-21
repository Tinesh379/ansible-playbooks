pipeline{
    agent {
        label 'linux'
    }
    options{
        buildDiscarder logRotator(artifactDaysToKeepStr: '1', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
        timeout(20)
    }
    environment{
        ANSIBLE=credentials('ansible')
    }
    parameters {
    booleanParam description: 'Enable to run the playbooks', name: 'RUN_PLAYBOOKS'
    string defaultValue: '<empty>', description: 'enter the name of playbook to run (ex: plabook.yml)', name: 'PLAYBOOK_NAME', trim: true
    }
    stages{
        stage('check ansible'){
            steps{
                sh 'ansible --version'
            }           
        }
        stage('run playbook'){
            when{
                expression{ params.RUN_PLAYBOOKS && params.PLAYBOOK_NAME!="<empty>"}
            }
            steps{
                sh '''
                ansible-playbook ${params.PLAYBOOK_NAME} -i hosts --extra-vars "ansible_user=$ANSIBLE_USR ansible_shh_pass=$ANSIBLE_PSW" -vv
                '''
            }
        }
    }
    post{
        always{
            cleanWs()
        }
    }

}