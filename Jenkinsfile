/* groovylint-disable CompileStatic, NoDef, VariableTypeRequired */
def mailRecipients = 'ludovic.smadja@jalios.com'
def imageName = 'registres.jalios.net/cloud/services/smtp'

properties(
    /* groovylint-disable-next-line DuplicateStringLiteral, LineLength */
    [buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '2', numToKeepStr: '2'))]
)

try {
    withEnv(['DOCKER_BUILDKIT=1']) {
        node('docker') {
            def imageTag = env.BRANCH_NAME
            echo "Building jalios docker image ${imageName}:${imageTag}"
            stage('Checkout source') {
                checkout scm
                dir('postfix_exporter') {
                    /* groovylint-disable-next-line LineLength */
                    checkout poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [[$class: 'CloneOption', depth: 1, noTags: true, reference: '', shallow: true]], userRemoteConfigs: [[url: 'https://github.com/kumina/postfix_exporter.git']]]
                }
            }

            if (imageName == null || imageName.trim() ==  '') {
                throw new IllegalStateException('imageName is not valid')
            }

            if (imageName == null || imageName.trim() ==  '') {
                imageTag = 'dev'
            }

            cacheParameter = '--no-cache=true'

            stage('Build') {
                thrownException = null
                try {
                    //pull(baseImage)
                    newImage = docker.build("${imageName}:${imageTag}", "--compress=true ${cacheParameter} .")
                    sh "docker push ${imageName}:${imageTag}"
                } catch (all) {
                    failBuild('Exception during build stage', all, currentBuild)
                    thrownException = all
                }
                finally {
                    echo 'cleaning workspace'
                    deleteDir()
                }
            }
        }
    }
}
catch (all) {
    failBuild('Docker image build', all, currentBuild)
}
stage('Post build') {
    echo 'Post build action'
    notifyByMail(mailRecipients)
}
