# Deploying S3 Object Storage with a stable A10 ADC config

## 11. Correct ADC Configuration

To stabilize behavior, the ADC must provide consistent backend affinity.

Server Configuration
```bash
slb server minio-lab 10.109.201.200
  port 9000 tcp
  port 9100 tcp
  port 9200 tcp
```

Service Group
```bash
slb service-group sg-minio-lab tcp
  method least-connection
  member minio-lab 9000
  member minio-lab 9100
  member minio-lab 9200
```
Persistence Template
```bash
slb template persist source-ip persist-minio
  timeout 30
```
Stable TCP Template
```bash
slb template tcp-proxy tp-stable-minio
  idle-timeout 1800
```
Virtual Server
```bash
slb virtual-server vs-minio-lab 10.108.200.201
  port 80 http
    source-nat auto
    service-group sg-minio-lab
    template persist source-ip persist-minio
    template tcp-proxy tp-stable-minio
```
## 12. Demonstrating Stable Behavior

Repeat the same test.
```bash
for i in $(seq 1 20); do
  echo "run $i"
  mc ls minio-vip/node3-only
done
```
Output remains consistent:
```
node3.txt
node3.txt
node3.txt
```
Meanwhile:
```bash
mc ls minio-vip/node1-only
```
Always fails.

This proves that the ADC now routes the client consistently to one backend node.