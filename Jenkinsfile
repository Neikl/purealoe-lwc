#!groovy
node {
    stage('Checkout Source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    stage('Souce Code Analysis'){

        sh "sonar-scanner \
  -Dsonar.projectKey=sonar-confluence-integration \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://192.168.0.103:9000 \
  -Dsonar.login=3319447c65d21d9aaa9f7ed1023fc226492d6462
    }
}
