# Installing ArgoCD on k3s

Load install yaml:

```sh
curl -L -O https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Add `--insecure` flag to command line of `argocd-server`.

```sh
kubectl create namespace argocd
kubectl apply -n argocd -f install.yaml
kubectl apply -n argocd -f ingress.yaml
```

## Client

```sh
brew install argocd
```

## DNS

Create DNS entries for `argocd.mytracks4mac.info` and `grpc.argocd.mytracks4mac.info`.

## Initial login

This is stored in secret `argocd-initial-admin-secret`. Get it:

```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Login using cli and change password:

```sh
argocd login grpc.argocd.mytracks4mac.info
argocd account update-password
```

## Add kubernetes cluster

```sh
argocd cluster add vps-01
```

## Add Guestbook demo application

Add an app without deploying it:

```sh
argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://vps-01.mytracks4mac.info:6443 --dest-namespace guestbook
```

Get status of the app:

```sh
argocd app get guestbook
```

Sync the app:

```sh
argocd app sync guestbook
```

Delete the app:
```sh
argocd app delete guestbook
```

## Add application using yaml file


