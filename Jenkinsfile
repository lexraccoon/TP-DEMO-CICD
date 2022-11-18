pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Recuperation des sources') {
            steps {
               git branch: 'main', url: 'https://github.com/ziberty/GoSecuri.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Execute') {
            steps {
                dir("target") {
                    sh 'java -jar goSecuri-1.0.jar'
                }
            }
        }
        /*stage('Send') {
            steps {
                sh 'scp -i ~/.ssh/id_rsa -r /var/jenkins_home/workspace/MSPR_APPLI/web gosecuri@192.168.220.134:/var/www/gosecuri_web'
            }
        }*/
        
    }
    post {
        always {
            junit '**/surefire-reports/*.xml'
            
			recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tool: checkStyle()
            recordIssues enabledForFailure: true, tool: spotBugs()
            recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
            recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
            
        }

    }
}
/*pipeline {
    agent any

    stages {
        stage('Package') {
            steps {
            	sh 'mvn clean'
                sh 'mvn package' 
            }
        }
        stage('Analyse') {
            steps {
            	sh 'mvn checkstyle:checkstyle'
                sh 'mvn spotbugs:spotbugs'
                sh 'mvn pmd:pmd' 
            }
        }
        stage('Publish') {
            steps {
                archiveArtifacts '/target/*.jar'
            }
        }
        
    }
}*/
