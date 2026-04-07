node {
    def appDir = "${WORKSPACE}" 

    // Isse Jenkins ko pata chalta hai ki GitHub se signal lena hai
    properties([pipelineTriggers([githubPush()])])

    stage('Clean Workspace'){
        deleteDir()
    }

    stage('Clone Repo'){
        git branch: 'main', url: 'https://github.com/techfaiyaz5/cicd-aws.git'
    }

    stage('Install & Build'){
        sh """
            npm install
            npm run build
        """
    }

    stage('Deploy with PM2'){
        sh """
            # Quotes ke saath path handle kiya hai
            pm2 delete "nestjs-app" || true
            pm2 start "${appDir}/dist/main.js" --name "nestjs-app"
            pm2 save
        """
    }
}