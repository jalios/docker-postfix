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
    }
}