pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '18.217.165.183', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.218.103.112', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
 
stages{
        stage('Build'){
            steps {
                sh '/opt/maven/apache-maven-3.5.4/bin/mvn clean package'
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
                       sh 'cp  **/target/*.war /opt/tomcat/apache-tomcat-8.5.32-staging/webapps'                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        sh 'cp **/target/*.war /opt/tomcat/apache-tomcat-8.5.32-prod/webapps'
                    }
                }
            }
        }
    }
}
