#README.md
# install ArgoCD in k8s
#link:  https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd
#link2 https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# access ArgoCD UI
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8080:443 -n argocd

# login with admin user and below token (as in documentation):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo

# you can change and delete init password


# application.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: myapp-argo-application
  namespace: argocd
spec:
  project: default

  source:
    repoURL: https://gitlab.com/nanuchi/argocd-app-config.git
    targetRevision: HEAD
    path: dev
  destination: 
    server: https://kubernetes.default.svc
    namespace: myapp

  syncPolicy:
    syncOptions:
    - CreateNamespace=true

    automated:
      selfHeal: true
      prune: true

 with the automated attribute, argocd will pull changed every 3 minutes. 
https://www.youtube.com/watch?v=MeU5_k9ssrs  review practical in video from 27 minutes.
https://www.linkedin.com/pulse/how-deploy-kubernetes-jenkins-gitops-github-pipeline-rajdeep-saha   good practicals.
