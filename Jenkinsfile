pipeline {
  agent any
  tools {
        maven "Maven 3.8.6" 
   }

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }  
       }
      stage('Test Maven - JUnit') {
            steps {
              sh "mvn test"
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
              }
            }
        }
   post {
        always {
            script {
                cest = TimeZone.getTimeZone("CEST")
                def cest = new Date()
                println(cest) 
                def mailRecipients = 'vsingh@zuelligpharma.com'
                def jobName = currentBuild.fullDisplayName
                env.Name = Jenkins
                env.cest = cest
                emailext body: '''${SCRIPT, template="email-html.template"}''',
                mimeType: 'text/html',
                subject: "[Jenkins] ${jobName}",
                to: "${mailRecipients}",
                replyTo: "${mailRecipients}"
              }
          }   
      }
    }
}
