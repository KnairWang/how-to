# generate key and crt file:
  ```bash
  openssl req -newkey ras:4096 -nodes -sha256 -keyout ./tls.key -x509 -days 3650 -out ./tls.crt -addext 'subjectAltName = IP:163.184.224.56'
  ```
  note: replace the value of `-days`, and replace IP address of `-addext`.

# registry deployment yaml template:
  ```yml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    annotations:
      deployment.kubernetes.io/revision: "7"
      field.cattle.io/creatorId: u-mhjdv3aue6
      field.cattle.io/publicEndpoints: '[{"addresses":["163.184.224.56"],"port":35000,"protocol":"TCP","serviceName":"zen:registry-nodeport","allNodes":true}]'
    creationTimestamp: "2020-07-22T01:28:21Z"
    generation: 10
    labels:
      cattle.io/creator: norman
      workload.user.cattle.io/workloadselector: deployment-zen-registry
    name: registry
    namespace: zen
    resourceVersion: "37079367"
    selfLink: /apis/apps/v1/namespaces/zen/deployments/registry
    uid: 08094c17-74d0-48da-b477-19ec2372e568
  spec:
    progressDeadlineSeconds: 600
    replicas: 1
    revisionHistoryLimit: 10
    selector:
      matchLabels:
        workload.user.cattle.io/workloadselector: deployment-zen-registry
    strategy:
      rollingUpdate:
        maxSurge: 1
        maxUnavailable: 0
      type: RollingUpdate
    template:
      metadata:
        annotations:
          cattle.io/timestamp: "2020-07-22T05:41:04Z"
          field.cattle.io/ports: '[[{"containerPort":5000,"dnsName":"registry-nodeport","hostPort":0,"kind":"NodePort","name":"registry","protocol":"TCP","sourcePort":35000}]]'
        creationTimestamp: null
        labels:
          workload.user.cattle.io/workloadselector: deployment-zen-registry
      spec:
        containers:
        - env:
          - name: REGISTRY_HTTP_TLS_CERTIFICATE
            value: /certs/tls.crt
          - name: REGISTRY_HTTP_TLS_KEY
            value: /certs/tls.key
          image: registry:2.7.1
          imagePullPolicy: Always
          name: registry
          ports:
          - containerPort: 5000
            name: registry
            protocol: TCP
          resources: {}
          securityContext:
            allowPrivilegeEscalation: false
            capabilities: {}
            privileged: false
            readOnlyRootFilesystem: false
            runAsNonRoot: false
          stdin: true
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
          tty: true
          volumeMounts:
          - mountPath: /var/lib/registry
            name: registry
          - mountPath: /certs
            name: certs
            readOnly: true
        dnsConfig: {}
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext: {}
        terminationGracePeriodSeconds: 30
        volumes:
        - name: registry
          persistentVolumeClaim:
            claimName: registry0
        - name: certs
          secret:
            defaultMode: 256
            optional: false
            secretName: trail-key
  status:
    availableReplicas: 1
    conditions:
    - lastTransitionTime: "2020-07-22T01:28:39Z"
      lastUpdateTime: "2020-07-22T01:28:39Z"
      message: Deployment has minimum availability.
      reason: MinimumReplicasAvailable
      status: "True"
      type: Available
    - lastTransitionTime: "2020-07-22T01:28:21Z"
      lastUpdateTime: "2020-07-22T05:41:21Z"
      message: ReplicaSet "registry-9db54bbbb" has successfully progressed.
      reason: NewReplicaSetAvailable
      status: "True"
      type: Progressing
    observedGeneration: 10
    readyReplicas: 1
    replicas: 1
  updatedReplicas: 1
  ```
# add ip to docker/certs.d
  ```bash
  cp tls.crt /etc/docker/certs.d/163.184.224.56:35000/
  ```
