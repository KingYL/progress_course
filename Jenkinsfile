node {
    stage('SCM') {
        git 'https://github.com/KingYL/progress_course.git'
    }
    stage('QA') {
        sh 'sonar-scanner -X'
    }
    stage('build') {
        def mvnHome = tool 'M3'
        sh "${mvnHome}/bin/mvn -B clean package"
    }
    stage('deploy') {
        sh "docker stop tomcatweb || true"
        sh "docker rm tomcatweb || true"
        sh "docker run --name tomcatweb -p 10086:8080 -v /root/webapps:/usr/local/tomcat/webapps -d tomcat"
        sh "cp target/MavenDemo.war /root/webapps"
    }
    stage('results') {
        archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
    }
}