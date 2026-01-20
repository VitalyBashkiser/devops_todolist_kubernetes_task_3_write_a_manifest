# Instructions

## 1) Build & push image
```bash
docker build -t user900002/todoapp:3.0.0 .
docker login
docker push user900002/todoapp:3.0.0
```

## 2) Apply manifests
```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml
```

## 3) Test ToDo app with port-forward
```bash
kubectl -n todoapp port-forward pod/todoapp 8000:8000
```

Open:
- http://localhost:8000/
- http://localhost:8000/api/
- http://localhost:8000/api/readiness
- http://localhost:8000/api/liveness

## 4) Test ToDo app using busyboxplus:curl
Get the pod IP:
```bash
kubectl -n todoapp get pod todoapp -o jsonpath='{.status.podIP}{"\n"}'
```

Start a shell in busybox:
```bash
kubectl -n todoapp exec -it busybox -- sh
```

Inside the container (replace `<POD_IP>`):
```bash
curl -i http://<POD_IP>:8000/
curl -i http://<POD_IP>:8000/api/readiness
curl -i http://<POD_IP>:8000/api/liveness
```
