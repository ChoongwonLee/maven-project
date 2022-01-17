pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
 
stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
 
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        // bat "winscp -i C:/Users/choongwon.a.lee/amazon-example.pem C:/ProgramData/Jenkins/.jenkins/workspace/FullyAutomated/webapp/target/webapp.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                        bat "copy -r C:/ProgramData/Jenkins/.jenkins/workspace/FullyAutomated/webapp/target/webapp.war C:/Tomcat/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        // bat "winscp -scp -i C:/Users/choongwon.a.lee/amazon-example.pem C:/ProgramData/Jenkins/.jenkins/workspace/FullyAutomated/webapp/target/webapp.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                        bat "copy -r C:/ProgramData/Jenkins/.jenkins/workspace/FullyAutomated/webapp/target/webapp.war C:/Tomcat-prod/webapps"
                    }
                }
            }
        }
    }
}