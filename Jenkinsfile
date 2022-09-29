node{
    def buildNumber = BUILD_NUMBER
    stage("Git Clone"){
        git url: 'https://github.com/athirumalairaja/java-docker-projrct.git',branch: 'master'
    }

    stage("Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      sh "${mavenHome}/bin/mvn clean package"
    }
    stage("Build Docker Image"){
        sh "docker build -t athirumalairaja/java-web-app-docker:${buildNumber} ."
    }
    stage("Push Docker Image"){
        withCredentials([string(credentialsId: 'DOCKER_PWD', variable: 'DOCKER_PWD')]) {
          sh "docker login -u athirumalairaja -p ${DOCKER_PWD}"
        }
        sh "docker push athirumalairaja/java-web-app-docker:${buildNumber}"
    }
    stage("Run Docker Image In Dev Server"){
         sshagent(['DOCKER_SERVER']) {
         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.33.233 docker rm -f javawebapp || true"
         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.33.233 docker run -d -p 8080:8080 --name javawebapp athirumalairaja/java-web-app-docker:${buildNumber}"
       }
       
    }
}
