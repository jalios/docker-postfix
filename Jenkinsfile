def baseTag = ""

node(){
    baseTag = env.BRANCH_NAME
}

withEnv(['DOCKER_BUILDKIT=1']) {
    DockerImage{
        pushLatestOnSuccess=false
        imageName="registres.jalios.net/cloud/services/smtp"
        imageTag="${baseTag}"
        ignoreCache=true
        mailRecipients= "ludovic.smadja@jalios.com"
        doAdditionalCheckout= {
            dir('postfix_exporter'){
                checkout poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [[$class: 'CloneOption', depth: 1, noTags: true, reference: '', shallow: true]], userRemoteConfigs: [[url: 'https://github.com/kumina/postfix_exporter.git']]]
            }
        }
    }
}