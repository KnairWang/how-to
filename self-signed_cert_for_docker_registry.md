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
    labels:
      app: registry
    name: registry
    namespace: zen
  spec:
    progressDeadlineSeconds: 600
    replicas: 1
    revisionHistoryLimit: 10
    strategy:
      rollingUpdate:
        maxSurge: 1
        maxUnavailable: 0
      type: RollingUpdate
    template:
      metadata:
        labels:
          app: registry
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
          volumeMounts:
          - mountPath: /var/lib/registry
            name: registry
          - mountPath: /certs
            name: certs
            readOnly: true
        restartPolicy: Always
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
  ```
# add ip to docker/certs.d
  ```bash
  cp tls.crt /etc/docker/certs.d/163.184.224.56:35000/
  ```
