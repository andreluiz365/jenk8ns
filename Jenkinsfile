def label = "kaniko-${UUID.randomUUID().toString()}"
podTemplate(name: 'kaniko', label: label, yaml: """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /root
      - name: context
        mountPath: /context  
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: jenkins-docker-cfg
          items:
            - key: .dockerconfigjson
              path: .docker/config.json
  - name: context
    emptyDir: {}                    
"""
  ) {
  node(label) {
    stage('Kaniko') {
      git 'https://github.com/jakelima18/forum-laravel-kubernetes.git'
      container(name: 'kaniko', shell: '/busybox/sh') {
          sh '''#!/busybox/sh
          /kaniko/executor -f Dockerfile --context=dir:///workspace --destination=jacksonlima91/forum-app:$BUILD_NUMBER --verbosity debug 
          '''
      }
    }
  }
}