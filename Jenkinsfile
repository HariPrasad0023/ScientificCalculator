pipeline {
    agent any

    environment {
        DOCKER_REGISTRY_CREDENTIALS = 'DockerHubCred'
        DOCKER_IMAGE_NAME = 'hariprasad0023/calculator'
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    submoduleCfg: [],
                    userRemoteConfigs: [[url: '']]
                ])
            }
        }

        stage('Build') {
            steps {
                dir('/home/hari/Desktop/Sem2/SPE_MiniProject') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('/home/hari/Desktop/Sem2/SPE_MiniProject') {
                    sh 'mvn test'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('/home/hari/Desktop/Sem2/SPE_MiniProject') {
                    script {

                        // Build Docker image
                        docker.build("${DOCKER_IMAGE_NAME}", '.')
                    }
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('', 'DockerHubCred') {
                        sh "docker tag ${DOCKER_IMAGE_NAME}:latest ${DOCKER_IMAGE_NAME}:latest"
                        sh "docker push ${DOCKER_IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    ansiblePlaybook(
                        playbook: 'deploy.yml',
                        inventory: 'inventory'
                    )
                }
            }
        }
    }
}