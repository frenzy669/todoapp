node {
   def commit_id
   stage('step a') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
      sh label: '', script: '''      
      npm install 
      npm test 
      '''
     }
   }
   stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'frenzy669') {
       def app = docker.build("frenzy669/docker-nodejs-demo:assaf-${commit_id}", 'basics').push()
     }
   }
   stage('docker run') {
     sh label: '', script: """
      docker run --rm -tid --name docker_test -p 3000 frenzy669/docker-nodejs-demo:assaf-${commit_id}
      docker kill docker_test
      """
     }
   }

