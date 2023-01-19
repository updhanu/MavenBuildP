def containerName="mavenbuild"
def tag="latest"
def dockerHubUser="updhanu"
def httpPort="8090" 
node{
    stage ('checkout code'){
       git credentialsId: 'a53ad38e-8893-4d90-9198-fa98d03da5e1', url: 'https://github.com/updhanu/MavenBuildP.git'
    }

    stage ('Build'){
        sh "mvn clean install -Dmaven.test.skip=true"
    }

 

    stage ('Test Cases Execution'){
        sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
    }

 

    stage ('Sonar Analysis'){
 //	sh 'mvn sonar:sonar -Dsonar.host.url=http://52.242.130.228:9000/ -Dsonar.login=c3c8ae7444bed76a466fd925fb71de1f6a6eafff'
    }

 

    stage ('Archive Artifacts'){
//         archiveArtifacts artifacts: 'target/*.war'
    }

    stage("Image Prune"){
         sh "docker image prune -a -f"
         }

    stage('Image Build'){
        sh "docker build -t $containerName:$tag  -t $containerName --pull --no-cache ."
        echo "Image build complete"
        }

        stage('Push to Docker Registry'){
            withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', passwordVariable: 'dockerPassword', usernameVariable: 'dockerUser')]) {
                    sh "docker login -u $dockerUser -p $dockerPassword"
                    sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
                    sh "docker push $dockerUser/$containerName:$tag"
                    echo "Image push complete"
        }
                }
        node('KubernetesVM'){
            stage('Run App'){
                    sh """
              #kubectl create deployment kubernetes-bootcamp1 --image=docker.io/updhanu/$containerName:$tag --port=8090
                       kubectl get pods
                    """
                }
        }

    stage ('Notification'){
        emailext (
              subject: "Job  Completed!",
              body: "Jenkins Pipeline Job for Maven Build got completed.",
              to: "itsmedhanu2k01@gmail.com "
            )
    }

//     stage ('Deployment'){
//         ansiblePlaybook colorized: true, disableHostKeyChecking: true, playbook: 'deploy.yml'
//     }
}
