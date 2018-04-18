<style>
.reveal section img { background:none; border:none; box-shadow:none; }
</style>

![](assets/brigade.png)

@fa[arrow-down]
+++

![](assets/brigadeex.png)

---

### Agenda

- Brigade
- Gateways
- Secrets
- Code - GitLab
- Storage
- Minio
- Links

---

### What is Brigade?

@fa[arrow-down]
+++

![](https://pbs.twimg.com/media/CV2rpOMVAAA0T8-.jpg)


---
### Brigade Architecture

![](https://raw.githubusercontent.com/Azure/brigade/master/docs/topics/img/design-overview.png)


---
![](https://upload.wikimedia.org/wikipedia/commons/c/c0/I_want_you.jpg)

---
# Gateway

- a gateway is an entity that generates events
- out of the box Brigade ships with GitHub and DockerHub gateways
- other examples, **brig** CLI tool behaves as a gateway with a `brig run`
- Anything could be a source of events, we want you to contribute

---
# Kubernetes Secret

- Secrets are namespaced objects; exist in the context of a namespace
- You can access them via a volume or an environment variable from a container running in a pod
- The secret data on nodes is stored in tmpfs volumes
- A per-secret size limit of 1MB exists
- The API server stores secrets as plaintext in etcd
- All **data** fields must be base64 encoded

@fa[arrow-down]
+++

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
  labels:
    app: myapp
type: Opaque
data:
  username: YWRtaW4=
  password: Qm91bGRlcgo=
```

@fa[arrow-down]
+++

```shell
$ kubectl create -f ./secret.yaml
secret "mysecret" created
```
@fa[arrow-down]
+++

Decode
```shell
$ echo "Qm91bGRlcgo=" | base64 -D
Boulder
```
---
# Brigade Secrets

- **Events** are **Secrets**
- Using the Kubernetes Secret API, Brigade events are specially labeled Secrets

@fa[arrow-down]
+++

Sample Brigade Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: example
  labels:
    heritage: brigade
    project: brigade-1234567890
    build: 01C1R2SYTYAR2WQ2DKNTW8SH08
    component: build
type: brigade.sh/build
data:
  event_provider: github
  event_type: push
  revision:
    commit: 6913b2703df943fed7a135b671f3efdafd92dbf3
    ref: master
  build_name: example
  project_id: brigade-1234567890
  build_id: 01C1R2SYTYAR2WQ2DKNTW8SH08
  payload: "{ 'foo': 'bar' }"
  script: "console.log('hello');"
```
@[1]
@[3]
@[5-7]

---
# References

https://kubernetes.io/docs/concepts/configuration/secret/

---

### Questions?


@fa[twitter gp-contact](@lucus_patrick)

@fa[github gp-contact](lukepatrick)