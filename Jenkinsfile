pipeline {
    
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultValue: '85.195.102.24', description: 'Staging Tomcat')
        string(name: 'dev_path', defaultValue: '/opt/apache-tomcat-9.0.8', description: 'Staging deployment path')
        string(name: 'tomcat_prod', defaultValue: '85.195.102.24', description: 'Production Tomcat')
        string(name: 'prod_path', defaultValue: '/opt/apache-tomcat-9.0.8-prod', description: 'Production deployment path')
    }

    triggers {
        pollSCM('* * * * *');
    }
    
    stages {
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        sh 'scp **/target/*.war root@${tomcat_dev}:${dev_path}/webapps'
                    }
                }
                stage('Deploy to Production') {
                    steps {
                        sh 'scp **/target/*.war root@${tomcat_prod}:${prod_path}/webapps'
                    }
                }
            }            
        }

    }

}