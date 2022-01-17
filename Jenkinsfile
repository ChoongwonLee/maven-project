pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '3.145.134.83', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.190.156.40', description: 'Production Server')
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
                        bat "pscp -scp -i C:/Users/choongwon.a.lee/amazon-example.pem C:/ProgramData/Jenkins/.jenkins/workspace/FullyAutomated/webapp/target/webapp.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                        // bat "copy -r **/target/*.war C:/Tomcat/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "pscp -scp -i C:/Users/choongwon.a.lee/amazon-example.pem C:/ProgramData/Jenkins/.jenkins/workspace/FullyAutomated/webapp/target/webapp.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                        // bat "copy -r **/target/*.war C:/Tomcat-prod/webapps"
                    }
                }
            }
        }
    }
}