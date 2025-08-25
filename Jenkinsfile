pipeline {    
    agent any
    stages {
        stage("github clone for ansible"){
            steps{
                git branch: 'main', url: 'https://github.com/RaksAniruddha/tomcat_intstalltion'
            }
        }
        stage("install tomcat"){
            steps{
                ansiblePlaybook credentialsId: 'ansible-user', installation: 'ansible2', inventory: 'inventory.ini', playbook: 'tomcat_install.yml', vaultTmpPath: ''
            }
        }
        stage('Checkout Code') {
            steps {
                git 'https://github.com/RaksAniruddha/addressbook-cicd-project'
            }
        }

        stage("Compile") {
            steps {
                sh 'mvn -B -e compile'
            }
        }

        stage("Test") {
            steps {
                sh 'mvn -B test'
            }
        }

        stage("Package") {
            steps {
                sh 'mvn -B package'
            }
        }

       stage('Deploy WAR to Tomcat') {
    steps {
        sshagent(['ssh']) {
            sh '''
                # Copy WAR to Tomcat webapps
                scp -o StrictHostKeyChecking=no target/addressbook.war ubuntu@13.200.137.44: /home/ubuntu/apache-tomcat-9.0.108/webapps/
                
                # Restart Tomcat
                ssh -o StrictHostKeyChecking=no ubuntu@13.200.137.44  "cd /home/ubuntu/apache-tomcat-9.0.108/bin && ./shutdown.sh || true && ./startup.sh"
            '''
        }
    }
}
    }
}


