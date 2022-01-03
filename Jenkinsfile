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
                bat 'cmd.exe mvn clean package'
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
                        bat "cmd.exe cp **/target/*.war %tomcat_dev%"
                        deploy adapters: [tomcat9(credentialsId: '345ab1bc-3e20-4adf-980f-b94e3ad77552', path: '', url: 'http://localhost:8090/')], contextPath: null, war: '**/*.war'
                 
                    }
                    
                }
                stage ('Deploy to Production') {
                    steps {
                        bat "cmd.exe cp **/target/*.war %tomcat_prod%"
                    }
                }
            }
        }
    }
}
