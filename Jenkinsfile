podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: centos
        image: centos:latest
        command:
        - sleep
        args:
        - 99d
      restartPolicy: Never
''') {
      node(POD_LABEL) {
            stage('k8s') {
              git 'https://github.com/mp125uml/week8.git'
              container('centos') {
                stage('deploy calculator') {
                    sh '''
                    pwd
                    ls /bin/test
                    echo "getting kubectl"
                    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                    echo "chmod"
                    chmod +x ./kubectl
                    echo "apply"
                    ./kubectl apply -f calculator.yaml -n devops-tools
                    ./kubectl apply -f hazelcast.yaml -n devops-tools
                    '''
                }
              }
            }

      }
    }
