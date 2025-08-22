Pipeline{
    agent any

    environment{
        IMAGE_NAME = "notes-app"
        IMAGE_TAG = "latest"
        DOCKER_HUB_USER = "rugved28"
    }
    stages{
        stage("Code clone"){
            steps {
                sh "whoami"
                git branch: 'main', url: 'https://github.com/RugvedBhalerao/django-notes-app.git'
            }
        }
        stage("Code Build"){
            steps{
                sh """
                echo "Building Docker image..."
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                """
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHubCreds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]){
                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG
                    docker push $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG
                    echo "Image pushed Successfully..."
                    """
                }
            }
        }

    }
}
