pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '3.144.69.251', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.217.150.56', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "pscp -scp -i C:/Users/choongwon.a.lee/amazon-example.pem **/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "pscp -scp -i C:/Users/choongwon.a.lee/amazon-example.pem **/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}