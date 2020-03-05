def tomcat = "3.20.234.17"
pipeline{
    agent any
            stages{
                stage("checkout"){
                    steps{
                        checkout([$class: 'GitSCM', 
    branches: [[name: '*/master']], 
    userRemoteConfigs: [[credentialsId: '5e9832ef-84b6-4a32-ae73-3f965aeae670', url: 'https://github.com/ViswaCompany/mavenrepo.git']]
])

                    }
                }
                stage("build"){
                    steps{
                        sh "/usr/local/src/apache-maven/bin/mvn package"

                    }
                }
                stage('sonar'){
                steps{
                        withSonarQubeEnv('sonar'){
                        sh "/usr/local/src/apache-maven/bin/mvn sonar:sonar"
                        }
                    }
                }
                
                stage('deploy'){
                steps{
                sh "ssh root@$tomcat systemctl stop tomcat"
                sh "scp target/*.war root@$tomcat:/var/lib/tomcat/webapps" 
                sh "ssh root@$tomcat systemctl start tomcat"
                }
            }


                
            }
}
