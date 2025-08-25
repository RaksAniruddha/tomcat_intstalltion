pipeline {    
    agent any
    stages {
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
        sshagent(['tomcat-sshh']) {
            sh '''
                # Copy WAR to Tomcat webapps
                scp -o StrictHostKeyChecking=no target/addressbook.war ubuntu@3.86.113.174:/home/ubuntu/apache-tomcat-9.0.108/webapps/
                
                # Restart Tomcat
                ssh -o StrictHostKeyChecking=no ubuntu@3.86.113.174 "cd /home/ubuntu/apache-tomcat-9.0.108/bin && ./shutdown.sh || true && ./startup.sh"
            '''
        }
    }
}
    }
}
