1. Navigate to Load balancer URL on console:
https://console.us-ashburn-1.oraclecloud.com/load-balancer/load-balancers/ocid1.loadbalancer.oc1.iad.aaaaaaaahm33phaw7rl3zd5j4keirx7edz253q3shckmeih2iezqxnmps2cq


2. Open the productpage

http://129.213.15.82/productpage

3. Apply Header based routing:
cd  cd header-based-routing/
kubectl apply -f virtual-service-reviews-test-v2.yaml

v1 - no stars
v2 - black stars
v3 - red stars

4. Login as Jason (jason/jason) - Notice the black stars
Logout as jason

5. canary deployment (Split all load between v1 {no stars} and v3 {red stars})
cd ../canary-release/
kubectl apply -f virtual-service-reviews-50-v3.yaml

Telemetry Demonstration:

1. Kubernetes dashboard:
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep oke-admin | awk '{print $1}')
kubectl proxy
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login

2. Generate Load:

while true; do
  curl -s https://2886795267-80-ollie02.environments.katacoda.com/productpage > /dev/null
  echo -n .;
  sleep 0.2
done

3. Grafana Dashboard:
http://129.213.15.82:15031/dashboard/db/istio-mesh-dashboard

4. Jaeger dashboard:
http://129.213.15.82:15032/

5. Proxy expose Kiali:
kubectl port-forward $(kubectl get po -n istio-system -l app=kiali -o jsonpath={.items..metadata.name}) 20001 -n istio-system
(admin/admin)
http://localhost:20001


6. Weave Dashboard:
kubectl port-forward $(kubectl get po -n weave --selector=name=weave-scope-app -o jsonpath={.items..metadata.name}) 4040 -n weave
http://localhost:4040
