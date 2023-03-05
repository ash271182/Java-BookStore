pipeline {
    agent any
    stages {
        stage('version control') {
            steps {
                git url: 'https://github.com/pluralsight-projects/Java-BookStore.git',
                    branch: 'master'
            }
        }
        stage('build the code') {
            steps {
                sh 'export PATH="/home/kubeadmin/jdk1.8.0_351/bin:$PATH" && mvn -X package'
            }
        }
        stage('archive the artifacts') {
            steps {
                archiveArtifacts onlyIfSuccessful: true,
                    artifacts: '**/target/bookstore.war',
                    allowEmptyArchive: false
            }
        }
        stage('build_image') {
            steps {
                sh 'cp /home/kubeadmin/.jenkins/workspace/BookStore/target/bookstore.war /home/kubeadmin/docker/'
                sh 'cd /home/kubeadmin/docker && docker build -t ash271182/bookstore .'
            }
        }
        stage('push_image') {
            steps {
                sh 'docker push ash271182/bookstore'
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl create deploy bookstore --image ash271182/bookstore'
                sh 'kubectl expose deploy bookstore --port 8080'
            }
        }
    }
}
