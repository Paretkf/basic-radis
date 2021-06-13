Basic Radis
- Key value DB
- Like a dictionary


redis-cli
- get / set
- ใช้ connect DB

```bash
$ kubectl exec -it client-util -n basic-redis -- bash
```
-it คือ interactive
client-util ต้องการ exec ที่ pod อะไร
-n basic-redis ที่ namespace อะไร
-- bash exec program อะไร