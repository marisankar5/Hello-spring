pipeline {
    agent any
    stages {
        stage('Preparation') {
            steps { // for display purposes
                // Get some code from a GitHub repository
                git 'https://github.com/marisankar5/Hello-spring.git'
                // Get the Maven tool.
                // ** NOTE: This 'M3' Maven tool must be configured
                // **       in the global configuration.
            }
        }
        stage('Build') {
            steps {
                script{
                   mvnHome = tool 'M3'
                // Run the maven build
                   withEnv(["MVN_HOME=$mvnHome"]) {
                    if (isUnix()) {
                        sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
                    } else {
                        bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
                    }
                }
                script
                    {
                        junit '**/target/surefire-reports/TEST-*.xml'
                        archiveArtifacts 'target/*.war'
                }
                }
            }
        }
        stage("Deploy") {
                steps {
                    sshagent(['Ansible']){
                sh"ansible-playbook -i /etc/ansible/hosts /etc/ansible/script.yaml -u root"
                }
                }
            }

        }
    }
