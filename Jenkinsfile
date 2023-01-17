node('') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
		 sh 'mvn sonar:sonar -Dsonar.host.url=http://52.242.130.228:9000/ -Dsonar.login=c3c8ae7444bed76a466fd925fb71de1f6a6eafff'
	}

	stage ('Archive Artifacts'){
		// archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage('Docker Build'){
        sh ' docker --version '
        sh ' docker build -t mavenbuild . '
    }
    stage('Create Container '){
        sh ' docker run -d -p 9000:8080  --name dockercontainer111 mavenbuild'
    }
	
	
// 	stage ('Deployment'){
// 		ansiblePlaybook colorized: true, disableHostKeyChecking: true, playbook: 'deploy.yml'
// 	}
	
	stage ('Notification'){
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got not completed !!!",
		      to: "itsmedhanu2k01@gmail.com"
		    )
	}
}
