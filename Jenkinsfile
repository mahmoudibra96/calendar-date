pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            git url: 'https://github.com/mahmoudibra96/calendar-date.git' , branch: 'main' , credentialsId: 'githubtoken'
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker images -a')
            sh(script: """
               docker build -t calnder_app .
            """)
         }
      }
            stage('Pushing image to docker hub')  {
          steps {
              echo "Workspace is $WORKSPACE"
              dir("$WORKSPACE") {
              script {
                  docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                      def image = docker.build('mahmoudibrahem125/calender_app:latest')
                      image.push()
                  }
              }
              }
          }
          
      }
           stage('Deploy to local kubernetes') {
        steps {
           script{
              kubernetesDeploy(configs: "k8s.yaml" , kubeconfigId: "87147743-7e89-4213-9b8c-dede4ebd6c75")
           }
         }
      }
   }
}