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
    steps {
        sh """
            cd ${appDir}
            
            # 1. Purana process kill karna (Port 3000 khali karne ke liye)
            sudo fuser -k 3000/tcp || true
            
            # 2. PM2 ko sudo ke saath chalana taaki global list mein dikhe
            # Agar 'next-app' chal raha hai toh restart, nahi toh start
            sudo pm2 restart next-app --update-env || sudo pm2 start npm --name "next-app" -- start
            
            # 3. Save karna taaki hamesha chalta rahe
            sudo pm2 save
        """
    }
}