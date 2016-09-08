node {
  def registry_server = "192.168.1.200:5000"
  def project_name = "ckan"

  stage('build') {
    checkout scm

    sh 'git rev-parse --short HEAD > .git/commit-id'
    def commit_id = readFile('.git/commit-id').trim()

    sh "sudo docker build -t $registry_server/$project_name:$commit_id ."
    sh "sudo docker tag $registry_server/$project_name:$commit_id $registry_server/$project_name:latest"
    sh "sudo docker push $registry_server/$project_name:latest"
  }

  stage('Publish') {
    sh "ansible-playbook -i ansible/host.txt ansible/deploy.yaml"
  }
}
