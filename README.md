Run Redis
===

1. Enter workshop dir
$ cd chapter01-basic-knowledges/1.4-basic-redis/01-run-redis 

2. Create k8s namespace
$ kubectl apply -f 00-namespace.yml
namespace/basic-redis created

3. Check if namespace has created
$ kubectl get ns
NAME              STATUS   AGE
basic-redis       Active   20s
default           Active   7h59m
ingress-nginx     Active   46m
kube-node-lease   Active   7h59m
kube-public       Active   7h59m
kube-system       Active   7h59m

4. Create redis deployment
$ kubectl apply -f 01-deployment.yml
deployment.apps/redis created

5. Check redis deployment has created
$ kubectl get po -n basic-redis
NAME                     READY   STATUS    RESTARTS   AGE
redis-577d58dd6c-g7brv   0/1     Running   0          26s

** Wait until the STATUS is Running

6. Create redis service
$ kubectl apply -f 02-service.yml 
service/redis created

7. Check redis service has created
$ kubectl get svc -n basic-redis
NAME    TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
redis   ClusterIP   None         <none>        6379/TCP   19s

8. Create client-util pod
$ kubectl apply -f 03-client-util.yml
pod/client-util created

9. Check if client-util pod has created
$ kubectl get po -n basic-redis
NAME                     READY   STATUS    RESTARTS   AGE
client-util              1/1     Running   0          96s
redis-577d58dd6c-g7brv   1/1     Running   0          3m54s

** Wait until the STATUS is Running

10. Exec into client-util pod
$ kubectl exec -it client-util -n basic-redis -- bash
root@client-util:/#

11. Run redis-cli to connect to redis
$ redis-cli -h redis
redis:6379>

12. Exit from redis-cli
$ exit
root@client-util:/#

12.1 test
$ SET "name" "rachata" 
$ GET name

13. Exit from client-util
# exit

14. Do not cleanup workshop, we will use it in next workshop


Use Redis cli
===

1. Exec into client-util pod
$ kubectl exec -it client-util -n basic-redis -- bash

2. Start redis-cli
$ redis-cli -h redis
redis:6379>

3. Use SET command to set mykey = myvalue
$ SET "mykey" "myvalue"
OK

4. Use GET to get value from key
$ GET "mykey"
"myvalue"

5. Use EXPIRE command to expire mykey in 10 seconds
$ EXPIRE "mykey" 10
(integer) 1

** Wait 10 seconds

6. Test get mykey when it is expired
$ GET "mykey"
(nil)

7. SET mykey2
$ SET "mykey2" "myvalue2"
OK

8. GET mykey2
$ GET "mykey2"
"myvalue2"

9. Use DEL command to delete mykey2
$ DEL "mykey2"
(integer) 1

10. GET mykey2 to see how it is deleted
$ GET "mykey2"
(nil)

11. SET mykey1 and mykey2
$ SET "mykey1" "value1"
$ SET "mykey2" "value2"

12. Use KEYS to list all keys using wildcard
$ KEYS "mykey*"
1) "mykey2"
2) "mykey1"

13. Exit from redis
$ exit

14. Exit from client-util
$ exit

15. Cleanup workshop
$ kubectl delete ns basic-redis