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
              git 'https://github.com/mp125uml/week8.git'
              container('gradle') {
                stage('test calculator') {
                    sh '''
                    chmod +x gradlew
		            ./gradlew acceptanceTest -Dcalculator.url=http://$CALCIP:8080
                '''
                }
              }
            }

      }
    }
