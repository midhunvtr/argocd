
Killercoda+Argocd

Step 1: Start a Kubernetes Cluster
In Killercoda, verify:
kubectl get nodes
Step 2: Install Argo CD
Create namespace:
kubectl create namespace argocd
Install Argo CD:
kubectl apply -n argocd -f \
https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Step 3: Expose Argo CD
Port-forward:
kubectl port-forward svc/argocd-server -n argocd 8080:443
Step 4: Install Argo CD CLI
Check:
argocd version
If missing:
curl -sSL -o argocd \
https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64

chmod +x argocd

mv argocd /usr/local/bin/
Step 5: Get Password
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d

Echo
Step 6: Login
argocd login localhost:8080 \
--username admin \
--password <password> \
--grpc-web \
--insecure
Verify

argocd account get-user-info

Step 7: Create an Application Repository

kubectl create namespace demo
Step 9: Create Argo CD Application
Tell Argo CD where Git is:
argocd app create nginx-app \
--repo https://github.com/<user>/<repo>.git \
--path . \
--dest-server https://kubernetes.default.svc \
--dest-namespace demo
Step 10: Sync Application
Deploy resources:
argocd app sync nginx-app
argocd app get nginx-app
Step 11: Verify Resources
kubectl get all -n demo
Enable Auto Sync
Instead of manually syncing:
All code copied to clipboard
argocd app set nginx-app \
--sync-policy automated






