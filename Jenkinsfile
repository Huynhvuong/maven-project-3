pipeline {
    agent any
    tools {
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_prod', defaultValue: '10.10.1.11', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving .......'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

                stage ("Deploy to Production"){
                    steps {
                        sh "ssh -i /home/vuong/.ssh/id_rsa.p -tt root@${params.tomcat_prod} && cd /var/lib/apache-tomcat-8.5.38-production/bin && sh startup.sh"
                        sh "scp -i /home/vuong/.ssh/id_rsa -v -o StrictHostKeyChecking=no **/target/*.war root@${params.tomcat_prod}:/var/lib/apache-tomcat-8.5.38-production/webapps"
                    }
              
        }
    }
}
