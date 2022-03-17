podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: centos
        image: centos
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
                stage('start calculator') {
                    sh '''
                    pwd
                    ls /bin/test
                    echo "getting kubectl"
                    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                    echo "chmod"
                    chmod +x ./kubectl
                    echo "apply"
                    ./kubectl apply -f calculator.yaml
                    ./kubectl apply -f hazelcast.yaml
                    '''
                }
                stage('test calculator') {
                    sh '''
                    /bin/test $(curl calculator-service:8080/sum?a=1\\&b=2) -eq 3
                '''
                }
              }
            }

      }
    }
