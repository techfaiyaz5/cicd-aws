node {
    // Ab hum Jenkins ke apne workspace mein kaam karenge taaki permission ka panga na ho
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
            cd ${appDir}
            npm install
            npm run build  # Ye 'dist' folder banayega
        """
    }

    stage('Deploy with PM2'){
        echo 'Deploying to EC2 using PM2'
        sh """
            cd ${appDir}
            
            # PM2 ko batana ki purana process hatao aur naya 'dist/main.js' se chalu karo
            pm2 delete nestjs-app || true
            pm2 start dist/main.js --name "nestjs-app"
            
            pm2 save
        """
    }
}