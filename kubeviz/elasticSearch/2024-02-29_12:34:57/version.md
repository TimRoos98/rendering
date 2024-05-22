```mermaid
flowchart
subgraph KubernetesAPI [KubernetesAPI]
elasticsearch-master-certs[/
Name: elasticsearch-master-certs
Data:
ca.crt: LS0tLS1CRUdJTiBDR...
<span style='color: blue;'>tls.key: LS0tLS1CRUdJTiBSU...
</span><span style='color: green;'>tls.key2: LS0tLS1CRUdJTiBSU...
</span><span style='color: red;'>tls.crt: LS0tLS1CRUdJTiBDR...
</span>/]:::UPDATED
elasticsearch-master-credentials[/
Name: elasticsearch-master-credentials
Labels:
heritage: Helm
release: elasticsearch
app: elasticsearch-master
<span style='color: blue;'>chart: elasticsearchV2
</span>Data:
password: YmNsTEMxOGtKcXdWM...
<span style='color: blue;'>username: ZWxhfaeafd=
</span>/]:::UPDATED
end
KubernetesAPI:::UPDATED
subgraph elasticsearch-idfkd-test [Pod: elasticsearch-idfkd-test
]
direction BT
subgraph config-elasticsearch-idfkd-test [CONFIG:
]
end
elasticsearch-idfkd-test-elasticsearch-gqozy-test(
elasticsearch-gqozy-test
)
elasticsearch-idfkd-test-proxy{"Ingress
Egress
"}
elasticsearch-idfkd-test-elasticsearch-gqozy-test --> elasticsearch-idfkd-test-proxy
elasticsearch-idfkd-test-elasticsearch-certs[("elasticsearch-certs
Type: Secret
")]
elasticsearch-idfkd-test-elasticsearch-certs -.-> elasticsearch-idfkd-test-elasticsearch-gqozy-test
end
subgraph elasticsearch-new [Pod: elasticsearch-new
]
direction BT
subgraph config-elasticsearch-new [CONFIG:
name: elasticsearch-new
DNSPolicy: Default
NameServers: 
HostNetwork: true
Restart Policy: Never
Termination Grace Period: 30S
]
end
elasticsearch-new-elasticsearch-gqozy-test(
elasticsearch-gqozy-test
Resources: &#40request/limits&#41
cpu: Default/Default
memory: Default/DefaultEnv: ELASTIC_PASSWORD: elasticsearch-mas...
Commands: sh, -c, #!/usr/bin/env bash -e
curl -XGET --fail --cacert /usr/share/elasticsearch/config/certs/tls.crt -u "elastic:${ELASTIC_PASSWORD}" https://'elasticsearch-master:9200/_cluster/health?wait_for_status=green&timeout=1s'
Image Pull Policy: IfNotPresent
)
elasticsearch-new-proxy{"Ingress
Egress
"}
elasticsearch-new-elasticsearch-gqozy-test --> elasticsearch-new-proxy
elasticsearch-new-elasticsearch-certs[/
Type: Secret
 secretName: elasticsearch-master-certs/]
elasticsearch-new-elasticsearch-certs -. "/usr/share/elasticsearch/config/certs" .-> elasticsearch-new-elasticsearch-gqozy-test
end
elasticsearch-new:::NEW

subgraph elasticsearch-master [StatefulSet: elasticsearch-master]
subgraph elasticsearch-master-config [Config:
"podManagementPolicy: <span style='color: blue;'>OrderedReady</span>"]
end
subgraph elasticsearch-master-pvc [Volume Claims:]
elasticsearch-master-newVolume-pvc[(
newVolume
)]:::NEW
elasticsearch-master-elasticsearch-master-pvc[(
elasticsearch-master
Name: elasticsearch-master
AccessModes:
<span style='color: green;'>ReadOnlyMany</span>
<span style='color: red;'>ReadWriteOnce</span>

Resources: 
requests: <span style='color: blue;'>storage: 20Gi
</span>)]:::UPDATED
end
elasticsearch-master-pvc:::UPDATED
subgraph elasticsearch-master-pod-0 [Pod: elasticsearch-master-pod
Replicas: <span style='color: red;'>3</span> <span style='color: green;'>4</span>

]
direction BT
subgraph config-elasticsearch-master-pod-0 [CONFIG:
SecurityContext: fsGroup: 1000
<span style='color: blue;'>runAsUser: 2000
</span>]
end
config-elasticsearch-master-pod-0:::UPDATED
elasticsearch-master-pod-0-elasticsearch(
elasticsearch
Env: cluster.initial_master_nodes: elasticsearch-mas...
node.roles: master,data,data_...
discovery.seed_hosts: elasticsearch-mas...
cluster.name: elasticsearch
network.host: 0.0.0.0
ELASTIC_PASSWORD: elasticsearch-mas...
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.key: /usr/share/elasti...
xpack.security.transport.ssl.certificate: /usr/share/elasti...
xpack.security.transport.ssl.certificate_authorities: /usr/share/elasti...
xpack.security.http.ssl.key: /usr/share/elasti...
xpack.security.http.ssl.certificate: /usr/share/elasti...
<span style='color: blue;'>xpack.security.http.ssl.enabled: false
</span><span style='color: red;'>xpack.security.http.ssl.certificate_authorities: /usr/share/elasti...
</span>):::UPDATED
elasticsearch-master-pod-0-proxy{"Ingress
Egress
"}
elasticsearch-master-pod-0-elasticsearch <--http: 9200
transport: 9300
readinessProbe: HTTP POST /:http
-->elasticsearch-master-pod-0-proxy
elasticsearch-master-pod-0-elasticsearch-certs[("elasticsearch-certs
Type: Secret
")]
elasticsearch-master-elasticsearch-master-pvc <-.-> elasticsearch-master-pod-0-elasticsearch

elasticsearch-master-pod-0-elasticsearch-certs -.-> elasticsearch-master-pod-0-elasticsearch
end
elasticsearch-master-pod-0:::UPDATED
end
elasticsearch-master:::UPDATED
elasticsearch-master-service([
Service Type: ClusterIP
Name: elasticsearch-master
])
elasticsearch-master-service <-->elasticsearch-master-pod-0-proxy
elasticsearch-master-headless-service([
Service Type: ClusterIP
Name: elasticsearch-master-headless
Labels:
heritage: Helm
release: elasticsearch
<span style='color: red;'>chart: elasticsearch
app: elasticsearch-master
</span>Annotations:
service.alpha.kubernetes.io/tolerate-unready-endpoints: true
<span style='color: green;'>newAnnotation: new
</span>PortMapping:
TCP: 9200:
, <span style='color: red;'>9300:
</span>, <span style='color: green;'>9201:
</span>, <span style='color: green;'>9202:
</span>
]):::UPDATED
elasticsearch-master-headless-service <-->elasticsearch-master-pod-0-proxy
elasticsearch-idfkd-test-proxy --> elasticsearch-master-service
elasticsearch-new-proxy --> elasticsearch-master-service
elasticsearch-idfkd-test-proxy --> elasticsearch-master-headless-service
elasticsearch-new-proxy --> elasticsearch-master-headless-service
subgraph Legend
    direction LR

    start1[ ] -->|Network Flow| stop1[ ]
    style start1 height:0px;
    style stop1 height:0px;
    start2[ ] -.->|Storage Flow| stop2[ ]
    style start2 height:0px;
    style stop2 height:0px; 
    subgraph KubernetesAPILegend
        direction LR
        Parallelogram[/ Secret /]
    end
    subgraph StatefulSet
        direction LR
        subgraph rootConfig[StatefulSet Specific Config]
        end
        subgraph Pod
        direction LR
        subgraph PodConfig[Pod Specific Config]
        end
        Proxy{Ingress
         Egress}
        Container(Container)
        Storage[( Storage )]
        end
    end
    Stadium([ Service ])
    NEW[ New Resource ]:::NEW
    Updated[ Updated Resource 
    Value: <span style='color: red;'>Old Value</span> <span style='color: green;'>New Value</span>]:::UPDATED
    EQUAL[ Unchanged Resource ]
    end
classDef DELETED stroke:#f00
classDef NEW stroke:#0f0
classDef UPDATED stroke:#00f
```