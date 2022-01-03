pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev',
        defaultValue:'C:/dev/apache-tomcat-9.0.56/webapps',
        description:'Staging Server : 8080')
        string(name: 'tomcat_prod',
        defaultValue: 'C:/dev/apache-tomcat-9.0.56-production/webapps',
        description:'Staging Server : 8090')
    }

    triggers {
        pollSCM('* * * * *') // Polling source control
    }

    stages {
        stage('Build') {
            steps{
                sh 'mvn clean package'
            }
            post{
                success {
                    echo "Now Archiving ..."
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        
        stage('Deployments') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        sh "cp **/target/*.war %tomcat_dev%"
                    }
                }
                stage ('Deploy to Production') {
                    steps {
                        sh "cp **/target/*.war %tomcat_prod%"
                    }
                }
            }
        }
    }
}
