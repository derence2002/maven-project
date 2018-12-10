pipeline {
    agent any
    tools{
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.173.168.170', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.54.105.103', description: 'Production Server')
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
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "scp -i D:\Dropbox\Work\devops\tomcat-demo.ppk **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i D:\Dropbox\Work\devops\tomcat-demo.ppk **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}