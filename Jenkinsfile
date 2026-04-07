node {
    def appDir = "${WORKSPACE}" 

    stage('Clean Workspace'){
        deleteDir()
    }

    stage('Clone Repo'){
        git branch: 'main', url: 'https://github.com/techfaiyaz5/cicd-aws.git'
    }

    stage('Install & Build'){
        echo 'Building NestJS App...'
        sh """
            npm install
            npm run build
        """
    }

    stage('Deploy with PM2'){
        echo 'Deploying to EC2...'
        sh """
            # Sabse pehle purana process hatao (Space handling ke saath)
            pm2 delete "nestjs-app" || true
            
            # Direct build file ko start karo bina CD kiye
            pm2 start "${appDir}/dist/main.js" --name "nestjs-app"
            
            pm2 save
        """
    }
}