node {
    def appDir = '/home/ubuntu/nestjs-app' // Ubuntu home directory use karna safe hai

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
            mkdir -p ${appDir}
            
            # Code copy karna (rsync is good)
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}
            
            cd ${appDir}
            npm install
            npm run build  # Ye 'dist' folder banayega
        """
    }

    stage('Deploy with PM2'){
        echo 'Deploying to EC2 using PM2'
        sh """
            cd ${appDir}
            
            # PM2 check karega agar app chal raha hai toh restart, nahi toh fresh start
            # NestJS ke liye 'dist/main.js' hi main file hoti hai
            pm2 delete nestjs-app || true
            pm2 start dist/main.js --name "nestjs-app" --update-env
            
            pm2 save
        """
    }
}