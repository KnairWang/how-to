# create secret
## with command line
```bash
kubectl create secret docker-registry <secret_name> \
  --docker-server=<your-registry-server> \
  --docker-username=<your-name> \
  --docker-password=<your-password> \
  [ --docker-email=<your-email> ]
```
note the email is optional

## with yaml
1. username and password string:
    ```yml
    {username}:{password}
    ```
    encode the string with base64, the result ref as {usr:psw} below

2. a JSON string include auth:
    ```json
    {
        "auths": {
            "{your-registry-server}": {
                "auth": "{usr:psw}",
                "password": "{password}",
                "username": "{username}"
            }
        }
    }
    ```
    encode the JSON with base64, the result ref as {auths} below

3. a yaml string
    ```yml
    apiVersion: v1
    data:
        .dockerconfigjson: {auths}
    kind: Secret
    metadata:
        name: {secret-name}
        namespace: {secret-namespace}
    type: kubernetes.io/dockerconfigjson
    ```
