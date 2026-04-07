node {
    def appDir = "${WORKSPACE}" 

    stage('Clean Workspace'){
        deleteDir()
    }

    stage('Clone Repo'){
        git branch: 'main', url: 'https://github.com/techfaiyaz5/cicd-aws.git'
    }

    stage('Install & Build'){
        sh "npm install && npm run build"
    }

    stage('Deploy with PM2'){
        echo 'Deploying to EC2 as ubuntu user...'
        sh """
            # Jenkins ko ubuntu user ban kar PM2 chalane do
            # Isse 'ubuntu' user ki pm2 list mein app dikhega
            sudo -u ubuntu pm2 delete "nestjs-app" || true
            sudo -u ubuntu pm2 start '${appDir}/dist/main.js' --name "nestjs-app"
            sudo -u ubuntu pm2 save
        """
    }
}