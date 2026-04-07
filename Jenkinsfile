node {
    def appDir = "${WORKSPACE}" 

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
        echo 'Deploying to EC2...'
        sh """
            # Purana process delete karo
            pm2 delete "nestjs-app" || true
            
            # SABSE ZAROORI: Single quotes (' ') ke andar Double quotes (" ") 
            # Isse PM2 ko poora rasta ek sath milega
            pm2 start '${appDir}/dist/main.js' --name "nestjs-app"
            
            pm2 save
        """
    }
}