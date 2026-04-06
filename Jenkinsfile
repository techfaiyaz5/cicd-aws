node {
    // Aapka application directory
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace'){
        echo 'Cleaning Jenkins Workspace'
        deleteDir()
    }

    stage('Clone Repo'){
        echo 'Cloning the latest repo from GitHub'
        // Maine aapka naya repo URL yahan daal diya hai
        git branch: 'main', url: 'https://github.com/techfaiyaz5/cicd-aws.git'
    }

    stage('Install & Build'){
        echo 'Installing dependencies and building Next.js app'
        sh """
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}
            
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
            # Purana process stop karke naya start karega taaki pipeline finish ho sake
            pm2 restart next-app || pm2 start npm --name "next-app" -- start
            
            # Isse settings save ho jayengi
            pm2 save
        """
    }
}