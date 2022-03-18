podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: gradle
        image: gradle:jdk8
        command:
        - sleep
        args:
        - 99d
      restartPolicy: Never
''') {
      node(POD_LABEL) {
            stage('gradle') {
             stage('start calculator') {
                    sh '''
                    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                    chmod +x ./kubectl
                    ./kubectl apply -f calculator.yaml
                    ./kubectl apply -f hazelcast.yaml
                    '''
                }
                stage('test calculator') {
                    sh '''
                    chmod +x gradlew
                    ./gradlew acceptanceTest -Dcalculator.url=http://calculator-service:8080
                '''
                }
              }
            }

      }
    }
