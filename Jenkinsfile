pipeline {
    agent any

    stages {
	stage('Build') {
   	    steps {
       	       sh '/Users/glu01/.local/bin/pipenv --python python3 sync'
   	    }
	}
        stage('Test') {
            steps {
               sh '/Users/glu01/.local/bin/pipenv run pytest'
            }
        }
        stage('Package') {
	    when{
		    anyOf{ branch "master" ; branch 'release' }
	    }
            steps {
               sh 'zip -r sbdl.zip lib'
            }
        }
	stage('Release') {
	   when{
	      branch 'release'
	   }
           steps {
              sh "scp -i ~/.ssh/id_ed25519 -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf \${whoami}@localhost:~/sbdl-qa/"

           }
        }
	stage('Deploy') {
	   when{
	      branch 'master'
	   }
           steps {
               sh "scp -i ~/.ssh/id_ed25519 -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf \${whoami}@localhost:~/sbdl-prod/"
           }
        }
    }
}
