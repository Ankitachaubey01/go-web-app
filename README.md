Architecture:

<img width="945" height="488" alt="image" src="https://github.com/user-attachments/assets/ab593a4d-478a-43c6-bef7-e78372e8eb81" />

1)Build multistage dockerfile

2) create kubernetes cluster
   eksctl create cluster --name demo-cluster1 --region us-east-1 --node-type t3.small

3)install ingress controller which will deploy lb and forward traffic to ingress

<img width="845" height="316" alt="image" src="https://github.com/user-attachments/assets/3119056d-8854-4cff-a804-a55d19ca3cd9" />

4)Install helm and create helm chart

curl -LO https://get.helm.sh/helm-v3.14.0-windows-amd64.zip
tar -xf helm-v3.14.0-windows-amd64.zip
move windows-amd64\helm.exe C:\Windows\System32

-Move yaml files from k8/maifest folder to helm folder in template path
-You can modify deployment file to take image tag value from value.yaml 

5)CI: create .github/workflow folder

ci.yaml will have steps for below:


<img width="1147" height="427" alt="image" src="https://github.com/user-attachments/assets/81b62a04-929f-4ebe-ad1a-aa08163b1da5" />

->Take any push trigger from github->
->Build go code->test quality->
->Build docker image and Push iyt to docker Hub->Once new iamge is pushed to dockerhub take taht tag and update tag in helm/value.yaml 
->once updated push latest changes and commit to git.

6) Go to Git and create secret token for docker_username and docker_password
   generate personal access token and create token for git


7) push ci to git and commit
   
  git add .
  git commit -m "Update tag in Helm chart"
  git push

<img width="1351" height="543" alt="image" src="https://github.com/user-attachments/assets/fe0f780a-4142-452b-ac6c-05f78d380688" />


8) Install argocd using cmd from argocd folder

kubectl get svc -n argocd

NAME                                      TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP      10.100.75.126    <none>                                                                    7000/TCP,8080/TCP            2m12s
argocd-dex-server                         ClusterIP      10.100.60.231    <none>                                                                    5556/TCP,5557/TCP,5558/TCP   2m12s
argocd-metrics                            ClusterIP      10.100.205.71    <none>                                                                    8082/TCP                     2m11s
argocd-notifications-controller-metrics   ClusterIP      10.100.52.97     <none>                                                                    9001/TCP                     2m10s
argocd-redis                              ClusterIP      10.100.78.170    <none>                                                                    6379/TCP                     2m9s
argocd-repo-server                        ClusterIP      10.100.71.10     <none>                                                                    8081/TCP,8084/TCP            2m9s
argocd-server                             LoadBalancer   10.100.231.155   aaea104572a6d4651af9d6ed7015b8fb-2139489296.us-east-1.elb.amazonaws.com   80:32384/TCP,443:32034/TCP   2m8s
argocd-server-metrics                     ClusterIP      10.100.107.233   <none>    

Use load balancer url "aaea104572a6d4651af9d6ed7015b8fb-2139489296.us-east-1.elb.amazonaws.com"  to access argocd 
To get password:

-kubectl get secret -n argocd

NAME                          TYPE     DATA   AGE
argocd-initial-admin-secret   Opaque   1      51m

-kubectl edit secret argocd-initial-admin-secret -n argocd

-decode password:

$ echo SzNRQW8yZDJHSlFqZVRvYQ== | base64 --decode
K3QAo2d2GJQjeToa

-ArgoCD view:

<img width="663" height="713" alt="image" src="https://github.com/user-attachments/assets/f83e0607-68fa-4a2b-b667-d71474c0bcae" />

<img width="1569" height="564" alt="image" src="https://github.com/user-attachments/assets/77bc8019-1e8b-4090-9118-accf29a6280f" />


# Go Web Application

This is a simple website written in Golang. It uses the `net/http` package to serve HTTP requests.

## Running the server

To run the server, execute the following command:

```bash
go run main.go
```

The server will start on port 8080. You can access it by navigating to `http://localhost:8080/courses` in your web browser.

## Looks like this


![Website](static/images/golang-website.png)


