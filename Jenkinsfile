node('testing'){
    stage('Initialize') {
    echo 'Initializing...'
	sh 'echo $(whoami)'
	sh 'sudo apt-get update'
	sh 'sudo apt-get install nodejs -y'
    // unlink after first run else pipeline will fail
	// sh 'sudo ln -s /usr/bin/nodejs /usr/bin/node'    
    sh '/usr/bin/node -v'
	sh 'sudo apt-get install npm -y'
	sh 'npm -v'
        
    }

    stage('Checkout') {
        echo 'Getting source code...'
        checkout scm
    }

    stage('Build') {
        echo 'Building dependencies...'
        sh 'npm i'
    }

    stage('Test') {
        echo 'Testing...'
        sh 'npm test'
    }

    stage('Publish') {
        echo 'Publishing Test Coverage...'
		publishHTML (target: [
			allowMissing: false,
			alwaysLinkToLastBuild: false,
			keepAll: true,
			reportDir: 'coverage/lcov-report',
			reportFiles: 'index.html',
			reportName: "Application Test Coverage"
		])
    }
}

node('staging'){
    stage('Initialize') {
        echo 'Initializing...'
	    sh 'echo $(whoami)'
	    sh 'sudo apt-get update'
	    sh 'sudo apt-get install nodejs -y'
        // unlink after first run else pipeline will fail
	    //sh 'sudo ln -s /usr/bin/nodejs /usr/bin/node'    
        sh '/usr/bin/node -v'
	    sh 'sudo apt-get install npm -y'
	    sh 'npm -v'
    }
    stage('Checkout') {
        echo 'Getting source code...'
        checkout scm
    }

    stage('PM2 Install') {
        echo 'Installing PM2 to run application as daemon...'
        sh "sudo npm install pm2 -g"
    }

    stage('Build') {
        echo 'Building dependencies...'
        sh 'npm i'
    }

    stage('Test') {
        echo 'Testing...'
        sh 'npm test'
    }

    stage('Run Application') {
        echo 'Stopping old process to run new process...'
        sh '''
        sudo npm run pm2-stop
        sudo npm run pm2-start
        '''
    }
}



