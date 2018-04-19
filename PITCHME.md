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
### Gateway

- a gateway is an entity that generates events
- out of the box Brigade ships with GitHub and DockerHub gateways
- other examples, **brig** CLI tool behaves as a gateway with a `brig run`
- Anything could be a source of events, we want you to contribute

---
![](https://upload.wikimedia.org/wikipedia/commons/c/c0/I_want_you.jpg)


---
### Kubernetes Secrets

- Secrets are namespaced objects; exist in the context of a namespace
- You can access them via a volume or an environment variable from a container running in a pod
- The secret data on nodes is stored in tmpfs volumes
@fa[arrow-down]
+++
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
### Brigade Secrets

- **Events** are **Secrets**
- Using the Kubernetes Secret API, Brigade events are specially labeled Secrets

@fa[arrow-down]
+++

### Sample Brigade Secret

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
```
@[1-2]
@[3-4]
@[5-6]
@[7-10]

@fa[arrow-down]
+++

```yaml
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
@[2]
@[3]
@[4-6]
@[7-9]
@[10]
@[11]

---
### GitLab Gateway

[github.com/lukepatrick/brigade-gitlab-gateway](https://github.com/lukepatrick/brigade-gitlab-gateway)

---?code=https://raw.githubusercontent.com/lukepatrick/brigade-gitlab-gateway/master/brigade-gitlab-gateway/cmd/brigade-gitlab-gateway/server.go&lang=golang&title=Gateway Executable

@[30](Define HTTP Path)
@[40-50](Define a handler, register events, and run a web endpoint)
@[89-98](Parse and handle a Push Event)

---?code=https://raw.githubusercontent.com/lukepatrick/brigade-gitlab-gateway/master/pkg/webhook/gitlab.go&lang=golang&title=Gateway Secret writer

@[6](Use Brigade Core to write Secrets)
@[64-75](Write Secret)

---
# Demo

---



---
### References

- [kubernetes.io/docs/concepts/configuration/secret](https://kubernetes.io/docs/concepts/configuration/secret)
- [github.com/lukepatrick/brigade-gitlab-gateway](https://github.com/lukepatrick/brigade-gitlab-gateway)
- [github.com/lukepatrick/brigade-bitbucket-gateway](https://github.com/lukepatrick/brigade-bitbucket-gateway)
- [github.com/lukepatrick/minio-brigade](https://github.com/lukepatrick/minio-brigade)
- [github.com/Azure/brigade](https://github.com/Azure/brigade)

---

### Questions?


@fa[twitter gp-contact](@lucus_patrick)

@fa[github gp-contact](lukepatrick)

---?image=assets/gitpitch-audience.jpg&opacity=50

# Thank you!