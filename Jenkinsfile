
pipeline {
    agent any

    environment {
        KUBECTL = 'D:\\DockerApp\\resources\\bin\\kubectl.exe'
        KUBECONFIG = 'C:\\Users\\POOVARASAN\\.kube\\config'
    }

    stages {
        stage('Create PersistentVolumeClaim') {
            steps {
                bat '"%KUBECTL%" apply -f pvc.yaml'
            }
        }

        stage('Create Pod') {
            steps {
                bat '"%KUBECTL%" apply -f pod.yaml'
            }
        }

        stage('Verify Storage') {
            steps {
                bat '"%KUBECTL%" wait --for=jsonpath={.status.phase}=Bound pvc/lab-pvc --timeout=180s'
                bat '"%KUBECTL%" wait --for=condition=Ready pod/storage-pod --timeout=180s'
                bat '"%KUBECTL%" exec storage-pod -- cat /data/test.txt'
                bat '"%KUBECTL%" get pvc lab-pvc'
            }
        }
    }
}
