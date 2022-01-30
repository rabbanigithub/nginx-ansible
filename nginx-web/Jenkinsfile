node {
    stage('SCM Checkout'){
        git branch: 'master', url: 'https://github.com/rabbanigithub/mm.git'
    }
    stage('Install and start NGINX on remote server'){
        def apt_update = 'sudo apt update'
        def apt_install = 'sudo apt install -y nginx default-jre'
        sshagent(['6a534226-e2cb-41a5-bd5b-24427216b285']) {
            sh "ssh -o StrictHostKeyChecking=no vagrant@10.11.12.90 ${apt_update}"
            sh "ssh -o StrictHostKeyChecking=no vagrant@10.11.12.90 ${apt_install}"
        }
    }
    stage('Copy web code to NGINX document root'){
        def createTmpDir = 'mkdir /tmp/tmpWebFiles'
        def tmpFileDir = '/tmp/tmpWebFiles'
        def mvWebFiles = 'sudo mv /tmp/tmpWebFiles/* /var/www/html/'
        def webFiles  = './html-web/* '
	def rmTmpDir = 'rm -fr /tmp/tmpWebFiles'

        sshagent(['6a534226-e2cb-41a5-bd5b-24427216b285']) {
            sh "ssh -o StrictHostKeyChecking=no vagrant@10.11.12.90 ${createTmpDir}"
            sh "scp -o StrictHostKeyChecking=no ${webFiles} vagrant@10.11.12.90:${tmpFileDir}"
            sh "ssh -o StrictHostKeyChecking=no vagrant@10.11.12.90 ${mvWebFiles}"
            sh "ssh -o StrictHostKeyChecking=no vagrant@10.11.12.90 ${rmTmpDir}"
        }
    }
    stage('Start and enable NGINX server'){
        def startNGINX = 'sudo systemctl start nginx'
        def enableNGINX = 'sudo systemctl enable nginx'
        sshagent(['6a534226-e2cb-41a5-bd5b-24427216b285']) {
            sh "ssh -o StrictHostKeyChecking=no vagrant@10.11.12.90 ${startNGINX}"
            sh "ssh -o StrictHostKeyChecking=no vagrant@10.11.12.90 ${enableNGINX}"
        }
    }
}

