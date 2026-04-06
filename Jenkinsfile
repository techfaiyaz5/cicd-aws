node {
    // Application directory define ki gayi hai
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
            # Directory ensure karna aur permissions dena
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}
            
            # 1. Purani build file delete karna (Zaroori for Auto-Reflect)
            rm -rf ${appDir}/.next
            
            # 2. Code copy karna (rsync)
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}
            
            # 3. Dependencies install aur fresh build banana
            cd ${appDir}
            npm install
            npm run build
        """
    }

    stage('Deploy with PM2'){
        echo 'Deploying to EC2 in Background using PM2'
        sh """
            cd ${appDir}
            
            # 4. PM2 restart with --update-env taaki naya code load ho jaye
            # Agar process nahi chal raha toh start karega, warna restart karega
            pm2 restart next-app --update-env || pm2 start npm --name "next-app" -- start
            
            # Settings save karna taaki reboot par chalu rahe
            pm2 save
        """
    }
}