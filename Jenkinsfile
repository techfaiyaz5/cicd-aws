node {
    // WORKSPACE ko quotes mein rakha hai taaki space wala issue solve ho jaye
    def appDir = "${WORKSPACE}" 

    stage('Clean Workspace'){
        echo 'Cleaning Jenkins Workspace'
        deleteDir()
    }

    stage('Clone Repo'){
        echo 'Cloning the latest repo from GitHub'
        git branch: 'main', url: 'https://github.com/techfaiyaz5/cicd-aws.git'
    }

    stage('Install & Build'){
        echo 'Installing dependencies and building NestJS app'
        sh """
            # Quotes use kiye hain taaki space handle ho sake
            cd "${appDir}"
            npm install
            npm run build
        """
    }

    stage('Deploy with PM2'){
        echo 'Deploying to EC2 using PM2'
        sh """
            cd "${appDir}"
            
            # PM2 process management
            pm2 delete nestjs-app || true
            pm2 start dist/main.js --name "nestjs-app"
            
            pm2 save
        """
    }
}