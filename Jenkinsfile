node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace'){
        echo 'Cleaning Jenkins Workspace'
        deleteDir()
    }

    stage('Clone Repo'){
        echo 'Cloning the latest repo from GitHub'
        git branch: 'main', url: 'https://github.com/techfaiyaz5/cicd-aws.git'
    }

    stage('Install & Build'){
        echo 'Installing dependencies and building Next.js app'
        sh """
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}
            
            # Purani build delete karna zaroori hai
            rm -rf ${appDir}/.next
            
            # Code copy karna
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}
            
            cd ${appDir}
            npm install
            npm run build
        """
    }

    stage('Deploy with PM2'){
        echo 'Deploying to EC2 in Background using PM2'
        sh """
            cd ${appDir}
            
            # Port 3000 khali karna
            sudo fuser -k 3000/tcp || true
            
            # PM2 ko sudo ke saath restart karna
            sudo pm2 restart next-app --update-env || sudo pm2 start npm --name "next-app" -- start
            
            # Save karna taaki reboot par chalu rahe
            sudo pm2 save
        """
    }
}