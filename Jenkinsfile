pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ShahzaibRao/cicd-projects.git']])
            }
        }
        stage('Install Maven') {
            steps {
                sh 'wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz'
                sh 'tar -xzvf /var/lib/jenkins/workspace/test/apache-maven-3.9.4-bin.tar.gz'
                sh 'sudo rm -rf apache-maven-3.9.4-bin.tar.*'
            }
        }
        stage('Compile Sample Application') {
            steps {
                dir ('/var/lib/jenkins/workspace/test/addressbook/addressbook_main'){
                sh '/var/lib/jenkins/workspace/test/apache-maven-3.9.4/bin/mvn compile'
            }
            }
        }
         stage('Test Sample Application') {
            steps {
                dir ('/var/lib/jenkins/workspace/test/addressbook/addressbook_main'){
                sh '/var/lib/jenkins/workspace/test/apache-maven-3.9.4/bin/mvn test'
            }
            }
        }
         stage('Package Sample Application') {
            steps {
                dir ('/var/lib/jenkins/workspace/test/addressbook/addressbook_main'){
                sh '/var/lib/jenkins/workspace/test/apache-maven-3.9.4/bin/mvn package'
            }
            }
        }
        stage('Tomcat Prerequeste Installation') {
            steps {
                sh 'sudo apt-get update'
                sh 'sudo apt-get install openjdk-17-jre -y'
            }
        }
         stage('Tomcat  Installation') {
            steps {
                sh 'sudo apt-get update'
                sh 'wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.80/bin/apache-tomcat-9.0.80.tar.gz'
                sh 'tar -xzvf apache-tomcat-9.0.80.tar.gz'
                sh 'sudo rm -rvf apache-tomcat-9.0.80.tar.*'
                //sh 'sudo chown -R shahzaib:shahzaib apache-tomcat-9.0.80'
                sh 'sudo cp tomcat-users.xml apache-tomcat-9.0.80/conf/tomcat-users.xml'
                sh 'sudo cp context.xml apache-tomcat-9.0.80/webapps/manager/META-INF/context.xml'
                sh 'sudo sed -i "s/8080/8081/g" apache-tomcat-9.0.80/conf/server.xml'
            }
        }
         stage('Deploy Sample Application To Tomcat Server') {
            steps {
                sh 'sudo cp addressbook/addressbook_main/target/addressbook.war apache-tomcat-9.0.80/webapps/'
                sh 'sudo apache-tomcat-9.0.80/bin/startup.sh'
            }
        }

    }
}
