node {
    def GRADLE_HOME = tool name: 'gradle-4.10.2', type: 'hudson.plugins.gradle.GradleInstallation'
    def REPO_URL = 'https://github.com/awsdevops009/jenkins-docker-splunk.git'
    def DOCKERHUB_REPO = 'dashpradeep/webapp'

    stage('Clone') {        
        git url: REPO_URL
    }

    stage('Verify Gradle Files') {
        sh 'ls -la'
        sh 'test -f build.gradle || { echo "build.gradle not found"; exit 1; }'
        sh 'test -f settings.gradle || { echo "settings.gradle not found"; exit 1; }'
    }

    stage('Check Gradle Tasks') {
        sh "${GRADLE_HOME}/bin/gradle tasks"
    }

    stage('Build') {
        sh "${GRADLE_HOME}/bin/gradle clean build"
        sh "ls -la build/libs/*.war || echo 'WAR file not found'"
        sh "echo ====================="
        sh "cp build/libs/*.war docker/webapp.war || echo 'WAR file not found'"
        sh "pwd"
        sh "ls -la"
        sh "ls -la ./docker"
    }

    stage ('Docker Build') {
        docker.withTool('docker-latest') {
            sh "printenv"
            sh "pwd"
            sh "ls -la"
            def image1 = docker.build("${DOCKERHUB_REPO}:${BUILD_NUMBER}", "./docker")
            def image2 = docker.build("${DOCKERHUB_REPO}:latest", "./docker")
        }
    }

    /*
    stage ('Docker Push') {
        docker.withTool('docker-latest') {
            withCredentials([usernamePassword(credentialsId: 'dockerhub',
                             usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh "docker login -u ${USERNAME} -p ${PASSWORD} https://index.docker.io/v1/"
                sh "docker push ${DOCKERHUB_REPO}:${BUILD_NUMBER}"
                sh "docker push ${DOCKERHUB_REPO}:latest"
            }            
        }
    }
    */
}
