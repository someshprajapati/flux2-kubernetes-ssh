# Flux2 on Kubernetes using SSH Keys (without PAT token)

### 1. Kubernetes
Get a Kubernetes Cluster. In this, I use Docker Desktop for MAC.
### 2. Installing Flux
```
S😎MESH[flux2-kubernetes-ssh (master)]~$ brew install fluxcd/tap/flux

S😎MESH[flux2-kubernetes-ssh (master)]~$ flux --version
flux version 2.0.0-rc.1

S😎MESH[flux2-kubernetes-ssh (master)]~$ flux check --pre
► checking prerequisites
✔ Kubernetes 1.25.9 >=1.20.6-0
✔ prerequisites checks passed
```

### 3. Setting up the Flux
Make sure you are pointing to the kubernetes cluster you want to use.
```
S😎MESH[flux2-kubernetes-ssh (master)]~$ kubectx
Switched to context "docker-desktop".

S😎MESH[flux2-kubernetes-ssh (master)]~$ kubectl get nodes
NAME             STATUS   ROLES           AGE    VERSION
docker-desktop   Ready    control-plane   5m3s   v1.25.9

S😎MESH[flux2-kubernetes-ssh (master)]~$ kgp -A
NAMESPACE     NAME                                     READY   STATUS    RESTARTS   AGE
kube-system   coredns-565d847f94-p6swf                 1/1     Running   0          3m30s
kube-system   coredns-565d847f94-qc8rf                 1/1     Running   0          3m30s
kube-system   etcd-docker-desktop                      1/1     Running   5          3m33s
kube-system   kube-apiserver-docker-desktop            1/1     Running   5          3m30s
kube-system   kube-controller-manager-docker-desktop   1/1     Running   5          3m28s
kube-system   kube-proxy-zqmcq                         1/1     Running   0          3m30s
kube-system   kube-scheduler-docker-desktop            1/1     Running   5          3m35s
kube-system   storage-provisioner                      1/1     Running   0          3m24s
kube-system   vpnkit-controller                        1/1     Running   0          3m23s


S😎MESH[flux2-kubernetes-ssh (master)]~$ kubectl get ns
NAME              STATUS   AGE
default           Active   5m34s
kube-node-lease   Active   5m36s
kube-public       Active   5m36s
kube-system       Active   5m36s


S😎MESH[flux2-kubernetes-ssh (master)]~$ flux bootstrap git --url=ssh://git@github.com/someshprajapati/flux2-kubernetes-ssh.git --branch=master  --private-key-file=/Users/someshp/.ssh/id_rsa_personal

► cloning branch "master" from Git repository "ssh://git@github.com/someshprajapati/flux2-kubernetes-ssh.git"
✔ cloned repository
► generating component manifests
✔ generated component manifests
✔ committed sync manifests to "master" ("0ab83628e0a0621d81b91ac2e0c236af31126aea")
► pushing component manifests to "ssh://git@github.com/someshprajapati/flux2-kubernetes-ssh.git"
► installing components in "flux-system" namespace
✔ installed components
✔ reconciled components
► determining if source secret "flux-system/flux-system" exists
► generating source secret
✔ public key: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDrXLssr6py/5qMmXE9YqtUHIayla2H07pX2wj83OQWdCqs7DAdU9IAiJhQ57zn0qlTsD4ZRFQKkG5cCD5toK7i3L81VZ7ZbozU2PEUPItdBwsg/QcgVjji/kkAS/rI2cVKMAgJwZmoUztH8/QovnPk15wVeEYQT889S0DA3d49jzA2NCF+g2AVZ0e9TQXaTezso6+eMgf9mSYywgRnjp96qWrUrMzZ/fZ0WyJChZAgZughHzmIxs8JkldEcpqcrw2wnNEt4MJlxi7CKENYue3xvVdgggc628YOyiQ4F6IZmhCPT0xWXLWRwbbAghj5GbmhiGggx/SIzYP2B+wnsEgYzM/9Dqmp56IrRac97QAX/A1m1kK6WLSaGOa640Ha/fMPmwAeCrSQImY1v93i0Z+AybZwL34CySikRdut4ZvUKnrprndOjeyCDGBwYtM1w6pvcqdeOokUhWe9YOki8WfcJPzR9nYLjeaz5x/vO6F0KiovmxTAaoyPK0a1YcGyl+Eeq8/EVt88vfGLvxSiGLMEhD4DH+BFe/V9xX5wCSl4cltIEVINoourj4HyJhqUayNZWyU1Hpvg5+9wgNvJzVudW6SRKrmO+HYyXYxYCMW7SdlcAeSG8P5EEPs0sYqtii6n7zlYCJf9wMOsWgByawtLmGhiIWJD2bBPauGlOUNpjQ==
Please give the key access to your repository: y
► applying source secret "flux-system/flux-system"
✔ reconciled source secret
► generating sync manifests
✔ generated sync manifests
✔ committed sync manifests to "master" ("4dc8bec9e9526e404f9a8fdaa8670172fda02679")
► pushing sync manifests to "ssh://git@github.com/someshprajapati/flux2-kubernetes-ssh.git"
► applying sync manifests
✔ reconciled sync configuration
◎ waiting for Kustomization "flux-system/flux-system" to be reconciled
✔ Kustomization reconciled successfully
► confirming components are healthy
✔ helm-controller: deployment ready
✔ kustomize-controller: deployment ready
✔ notification-controller: deployment ready
✔ source-controller: deployment ready
✔ all components are healthy


S😎MESH[flux2-kubernetes-ssh (master)]~$ k get ns
NAME              STATUS   AGE
default           Active   6m11s
flux-system       Active   106s
kube-node-lease   Active   6m13s
kube-public       Active   6m13s
kube-system       Active   6m13s
```

After this step the below files got created in repo:
https://github.com/someshprajapati/flux2-kubernetes-ssh/tree/master/flux-system

Pull the flux system related files from remote repo which created by flux bootstrap.
```
S😎MESH[flux2-kubernetes-ssh (master)]~$ ll
total 48
-rw-r--r--@ 1 someshp  staff    21K May  9 21:12 README.md

S😎MESH[flux2-kubernetes-ssh (master)]~$ git pull
 create mode 100644 flux-system/gotk-components.yaml
 create mode 100644 flux-system/gotk-sync.yaml
 create mode 100644 flux-system/kustomization.yaml

S😎MESH[flux2-kubernetes-ssh (master)]~$ ls -l flux-system/
total 752
-rw-r--r--@ 1 someshp  staff   366K May  9 21:13 gotk-components.yaml
-rw-r--r--@ 1 someshp  staff   554B May  9 21:13 gotk-sync.yaml
-rw-r--r--@ 1 someshp  staff   115B May  9 21:13 kustomization.yaml


S😎MESH[flux2-kubernetes-ssh (master)]~$ kubectl get ns
NAME              STATUS   AGE
default           Active   25m
flux-system       Active   21m
kube-node-lease   Active   25m
kube-public       Active   25m
kube-system       Active   25m

S😎MESH[flux2-kubernetes-ssh (master)]~$ kgp -n flux-system
NAME                                       READY   STATUS    RESTARTS   AGE
helm-controller-746889d448-grf6r           1/1     Running   0          21m
kustomize-controller-99c4d84bd-6x5f2       1/1     Running   0          21m
notification-controller-66d964799d-4jwcz   1/1     Running   0          21m
source-controller-7ccc5d646-tfqbh          1/1     Running   0          21m
```

### 4. Creating Staging sources
```
S😎MESH[flux2-kubernetes-ssh (master)]~$ export GITHUB_ORG=someshprajapati

flux create source git staging \
    --url https://github.com/$GITHUB_ORG/flux-staging \
    --branch master \
    --interval 30s \
    --export \
    | tee apps/staging.yaml

flux create kustomization staging \
    --source staging \
    --path "./" \
    --prune true \
    --validation client \
    --interval 10m \
    --export \
    | tee -a apps/staging.yaml
```

> Results:
```
S😎MESH[flux2-kubernetes-ssh (master)]~$ flux create source git staging \
>     --url https://github.com/$GITHUB_ORG/flux-staging \
>     --branch master \
>     --interval 30s \
>     --export \
>     | tee apps/staging.yaml
---
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: staging
  namespace: flux-system
spec:
  interval: 30s
  ref:
    branch: master
  url: https://github.com/someshprajapati/flux-staging


S😎MESH[flux2-kubernetes-ssh (master)]~$ flux create kustomization staging \
>     --source staging \
>     --path "./" \
>     --prune true \
>     --validation client \
>     --interval 10m \
>     --export \
>     | tee -a apps/staging.yaml
Flag --validation has been deprecated, this arg is no longer used, all resources are validated using server-side apply dry-run
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: staging
  namespace: flux-system
spec:
  interval: 10m0s
  path: ./
  prune: true
  sourceRef:
    kind: GitRepository
    name: staging
```

### 5. Creating devops-toolkit sources
```
flux create source git \
    devops-toolkit \
    --url https://github.com/vfarcic/devops-toolkit \
    --branch master \
    --interval 30s \
    --export \
    | tee apps/devops-toolkit.yaml
```

> Results:
```
S😎MESH[flux2-kubernetes-ssh (master)]~$ flux create source git \
>     devops-toolkit \
>     --url https://github.com/vfarcic/devops-toolkit \
>     --branch master \
>     --interval 30s \
>     --export \
>     | tee apps/devops-toolkit.yaml
---
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: devops-toolkit
  namespace: flux-system
spec:
  interval: 30s
  ref:
    branch: master
  url: https://github.com/vfarcic/devops-toolkit
```


### 6. Add the newly created files in steps [4,5] to github repo
```
S😎MESH[flux2-kubernetes-ssh (master)]~$ git status

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	apps/devops-toolkit.yaml
	apps/staging.yaml


S😎MESH[flux2-kubernetes-ssh (master)]~$ git add apps/devops-toolkit.yaml apps/staging.yaml

S😎MESH[flux2-kubernetes-ssh (master)]~$ git commit -m "Added the app files" --author=someshprajapati

S😎MESH[flux2-kubernetes-ssh (master)]~$ git push
```

### 7. Once push the code to remote repo, run the flux.
```
S😎MESH[flux2-kubernetes (master)]~$ watch flux get sources git

Every 2.0s: flux get sources git                                                                                                             someshp-mbp: Tue May  9 21:26:46 2023

NAME            REVISION                SUSPENDED       READY   MESSAGE
flux-system     master@sha1:eade6ad7    False           True    stored artifact for revision 'master@sha1:eade6ad7'
```

### 8. Create empty github remote repo and clone locally
```
S😎MESH[Personal_github]~$ git clone git@github.com-personal:someshprajapati/flux-staging.git
S😎MESH[Personal_github]~$ mkdir -p flux-staging/apps
```


### 9. Create new namespaces `staging`
```
S😎MESH[flux2-kubernetes-ssh (master)]~$ kubectl create namespace staging
namespace/staging created

S😎MESH[flux2-kubernetes-ssh (master)]~$ k get ns
NAME              STATUS   AGE
default           Active   46m
flux-system       Active   41m
kube-node-lease   Active   46m
kube-public       Active   46m
kube-system       Active   46m
staging           Active   3s
```

### 10. Now we can see the flux is in sync for remote repo `flux-staging`
```
S😎MESH[Personal_github]~$ watch flux get sources git
Every 2.0s: flux get sources git                                                                                                             someshp-mbp: Wed May  3 22:01:12 2023

NAME            REVISION                SUSPENDED       READY   MESSAGE
devops-toolkit  master@sha1:96218231    False           True    stored artifact for revision 'master@sha1:96218231'
flux-system     master@sha1:cd4f22ed    False           True    stored artifact for revision 'master@sha1:cd4f22ed'
production      master@sha1:0a2f768a    False           True    stored artifact for revision 'master@sha1:0a2f768a'
staging         master@sha1:569d9b99    False           True    stored artifact for revision 'master@sha1:569d9b99'


S😎MESH[Personal_github]~$ watch flux get kustomizations
Every 2.0s: flux get kustomizations                                                                                                          someshp-mbp: Tue May  9 21:36:41 2023

NAME            REVISION                SUSPENDED       READY   MESSAGE

flux-system     master@sha1:4dc8bec9    False           False   failed to decode Kubernetes YAML from /tmp/kustomization-2524774858/staging.yaml: MalformedYAMLError: yaml: line 1
2: could not find expected ':' <nil>
```

### 11. Deploying the first release in staging
```
S😎MESH[Personal_github]~$ cd ..
S😎MESH[Personal_github]~$ cd flux-staging

echo "image:
    tag: 2.9.9
ingress:
    host: staging.devops-toolkit.$INGRESS_HOST.nip.io" \
    | tee values.yaml

flux create helmrelease \
    devops-toolkit-staging \
    --source GitRepository/devops-toolkit \
    --values values.yaml \
    --chart "helm" \
    --target-namespace staging \
    --interval 30s \
    --export \
    | tee apps/devops-toolkit.yaml
```

> Results:
```
S😎MESH[flux-staging (master)]~$ export INGRESS_HOST=192.168.20.22

S😎MESH[flux-staging (master)]~$ echo "image:
>     tag: 2.9.9
> ingress:
>     host: staging.devops-toolkit.$INGRESS_HOST.nip.io" \
>     | tee values.yaml
image:
    tag: 2.9.9
ingress:
    host: staging.devops-toolkit.192.168.20.22.nip.io


S😎MESH[flux-staging (master)]~$ flux create helmrelease \
>     devops-toolkit-staging \
>     --source GitRepository/devops-toolkit \
>     --values values.yaml \
>     --chart "helm" \
>     --target-namespace staging \
>     --interval 30s \
>     --export \
>     | tee apps/devops-toolkit.yaml
---
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: devops-toolkit-staging
  namespace: flux-system
spec:
  chart:
    spec:
      chart: helm
      reconcileStrategy: ChartVersion
      sourceRef:
        kind: GitRepository
        name: devops-toolkit
  interval: 30s
  targetNamespace: staging
  values:
    image:
      tag: 2.9.9
    ingress:
      host: staging.devops-toolkit.192.168.20.22.nip.io


S😎MESH[flux-staging (master)]~$ git status
Untracked files:
  (use "git add <file>..." to include in what will be committed)
    apps/
	values.yaml

S😎MESH[flux-staging (master)]~$ rm values.yaml

S😎MESH[flux-staging (master)]~$ git status
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   apps/devops-toolkit.yaml

S😎MESH[flux-staging (master)]~$ git commit -m "Initial commit"
[master f806b35] Initial commit
 1 file changed, 21 insertions(+)
 create mode 100644 apps/devops-toolkit.yaml

S😎MESH[flux-staging (master)]~$ git push
```

### 12. Verifying the first release in staging
```
S😎MESH[flux-staging (master)]~$ watch flux get sources git

S😎MESH[flux-staging (master)]~$ watch flux get helmreleases
Every 2.0s: flux get helmreleases                                                                                                            someshp-mbp: Wed May  3 22:23:50 2023
NAME                    REVISION        SUSPENDED       READY   MESSAGE
devops-toolkit-staging                  False           Unknown Reconciliation in progress


S😎MESH[flux-staging (master)]~$ watch flux get helmreleases
Every 2.0s: flux get helmreleases                                                                                                            someshp-mbp: Wed May  3 22:25:33 2023
NAME                    REVISION        SUSPENDED       READY   MESSAGE
devops-toolkit-staging  0.1.0           False           True    Release reconciliation succeeded


S😎MESH[flux-staging (master)]~$ kubectl get pods -n staging
NAME                                                             READY   STATUS    RESTARTS   AGE
staging-devops-toolkit-staging-devops-toolkit-5bccb778b8-jlrzx   1/1     Running   0          2m49s

S😎MESH[flux-staging (master)]~$ kubectl describe pod staging-devops-toolkit-staging-devops-toolkit-5bccb778b8-jlrzx -n staging | grep Image
    Image:          vfarcic/devops-toolkit-series:2.9.9
    Image ID:       docker-pullable://vfarcic/devops-toolkit-series@sha256:6324c8f0da5dbb0ef0a6f1a0d9c37a2a60070ef37e1344ae1fddd83e88805a4e
```

### 13. Verifying the second release in staging after new image update
```

S😎MESH[flux-staging (master)]~$ vi apps/devops-toolkit.yaml

S😎MESH[flux-staging (master)]~$ git diff
diff --git a/apps/devops-toolkit.yaml b/apps/devops-toolkit.yaml
index 6eae1de..e3cce8f 100644
--- a/apps/devops-toolkit.yaml
+++ b/apps/devops-toolkit.yaml
@@ -16,6 +16,6 @@ spec:
   targetNamespace: staging
   values:
     image:
-      tag: 2.9.9
+      tag: 2.9.17
     ingress:
       host: staging.devops-toolkit.192.168.20.22.nip.io


S😎MESH[flux-staging (master)]~$ git commit -am "Updated the tags"

S😎MESH[flux-staging (master)]~$ git push

S😎MESH[flux-staging (master)]~$ k get pods -n staging
NAME                                                             READY   STATUS              RESTARTS   AGE
staging-devops-toolkit-staging-devops-toolkit-5bccb778b8-jlrzx   1/1     Running             0          6m34s
staging-devops-toolkit-staging-devops-toolkit-76b6cb8665-dz2dn   0/1     ContainerCreating   0          2s


S😎MESH[flux-staging (master)]~$ k get pods -n staging
NAME                                                             READY   STATUS    RESTARTS   AGE
staging-devops-toolkit-staging-devops-toolkit-76b6cb8665-dz2dn   1/1     Running   0          50s

S😎MESH[flux-staging (master)]~$ k describe pod staging-devops-toolkit-staging-devops-toolkit-76b6cb8665-dz2dn -n staging |grep Image
    Image:          vfarcic/devops-toolkit-series:2.9.17
    Image ID:       docker-pullable://vfarcic/devops-toolkit-series@sha256:0b0ce138b002455f0620228437f220e904244c12ce088459f1703c17fa6ffa6b
```
