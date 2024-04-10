```mermaid
flowchart
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
        ParallelogramAlt[\ ConfigMap \]
        Parallelogram[/ Secret /]
    end
    subgraph Deployment
        direction LR
        subgraph DeploymentConfig[Deployment Specific Config]
        end
        Proxy2{Ingress
         Egress}
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
      
DELETED[ Deleted Resource ]:::DELETED
    NEW[ New Resource ]:::NEW
    Updated[ Updated Resource 
    Value: <span style='color: red;'>Old Value</span> <span style='color: green;'>New Value</span>]:::UPDATED
    EQUAL[ Unchanged Resource ]
    end
external["External Network"]
external --nodeIP:UDP-Dynamic 30000-32767 --> my-pihole-dhcp
external --nodeIP:UDP-Dynamic 30000-32767 TCP-Dynamic 30000-32767 --> my-pihole-dns-udp
subgraph KubernetesAPI [KubernetesAPI]
my-pihole-password[/
Name: my-pihole-password
Labels:
app: pihole
heritage: Helm
<span style='color: orange;'>release: my-piholeV2
</span><span style='color: green;'>updated: true
</span><span style='color: red;'>chart: pihole-2.22.0
</span>Data:
<span style='color: orange;'>password: NewPassword=
</span>/]:::UPDATED
my-pihole-custom-dnsmasq[\
Name: my-pihole-custom-dnsmasq
Labels:
app: pihole
chart: pihole-2.22.0
heritage: Helm
<span style='color: orange;'>release: my-piholeV2
</span>\]:::UPDATED
end
KubernetesAPI:::UPDATED
subgraph my-pihole-smoke-test [Pod: my-pihole-smoke-test
]
direction BT
subgraph config-my-pihole-smoke-test [CONFIG:
RestartPolicy: <span style='color: red;'>Never</span> <span style='color: green;'>OnFailure</span>
Termination Grace Period: <span style='color: red;'>0</span> <span style='color: green;'>20</span>
]
end
config-my-pihole-smoke-test:::UPDATED
my-pihole-smoke-test-hook1-container(
hook1-container
Image: <span style='color: red;'>curlimages/curl</span> <span style='color: green;'>curlimages/curlV2</span>
Commands: sh, <span style='color: red;'>-c</span>, curl http://my-pihole-web:80/, <span style='color: green;'>somethingTestV2</span>
ImagePullPolicy: <span style='color: red;'>IfNotPresent</span> <span style='color: green;'>Always</span>
):::UPDATED
my-pihole-smoke-test-proxy{"Ingress
Egress
"}
my-pihole-smoke-test-hook1-container --> my-pihole-smoke-test-proxy
end
my-pihole-smoke-test:::UPDATED
subgraph my-pihole [Deployment: my-pihole]
subgraph my-pihole-config [Config:
StrategyType: <span style='color: red;'>RollingUpdate</span> <span style='color: green;'>Recreate</span>
]
end
subgraph my-pihole-pod-0 [Pod: my-pihole-pod-0
]
direction BT
subgraph config-my-pihole-pod-0 [CONFIG:
NameServers: <span style='color: red;'>127.0.0.1</span>, <span style='color: green;'>0.0.0.0</span>, 8.8.8.8
]
end
config-my-pihole-pod-0:::UPDATED
pod-0-pihole(
pihole
Resources: &#40request/limits&#41
CPU: Default/<span style='color: red;'>Default</span>,<span style='color: green;'>1Gi</span>
Memory: <span style='color: red;'>Default</span>,<span style='color: green;'>500m</span>/<span style='color: red;'>Default</span>,<span style='color: green;'>1100m</span>
):::UPDATED
my-pihole-pod-0-proxy{"Ingress
Egress
"}
my-pihole-pod-0-pihole <--http: 80
dns: 53
dns-udp: 53
https: 443
client-udp: 67
readinessProbe: HTTP GET /admin/index.php:http
livenessProbe: HTTP GET /admin/index.php:http
-->my-pihole-pod-0-proxy
my-pihole-pod-0-config[("config
Type: EmptyDir
")]
my-pihole-pod-0-custom-dnsmasq[("custom-dnsmasq
Type: ConfigMap
")]
my-pihole-pod-0-config <-.-> my-pihole-pod-0-pihole

my-pihole-pod-0-custom-dnsmasq <-.-> my-pihole-pod-0-pihole

my-pihole-pod-0-custom-dnsmasq <-.-> my-pihole-pod-0-pihole
end
my-pihole-pod-0:::UPDATED
subgraph my-pihole-pod-1 [Pod: my-pihole-pod-1
]
direction BT
subgraph config-my-pihole-pod-1 [CONFIG:
NameServers: <span style='color: red;'>127.0.0.1</span>, <span style='color: green;'>0.0.0.0</span>, 8.8.8.8
]
end
config-my-pihole-pod-1:::UPDATED
pod-1-pihole(
pihole
Resources: &#40request/limits&#41
CPU: Default/<span style='color: red;'>Default</span>,<span style='color: green;'>1Gi</span>
Memory: <span style='color: red;'>Default</span>,<span style='color: green;'>500m</span>/<span style='color: red;'>Default</span>,<span style='color: green;'>1100m</span>
):::UPDATED
my-pihole-pod-1-proxy{"Ingress
Egress
"}
my-pihole-pod-1-pihole <--http: 80
dns: 53
dns-udp: 53
https: 443
client-udp: 67
readinessProbe: HTTP GET /admin/index.php:http
livenessProbe: HTTP GET /admin/index.php:http
-->my-pihole-pod-1-proxy
my-pihole-pod-1-config[("config
Type: EmptyDir
")]
my-pihole-pod-1-custom-dnsmasq[("custom-dnsmasq
Type: ConfigMap
")]
my-pihole-pod-1-config <-.-> my-pihole-pod-1-pihole

my-pihole-pod-1-custom-dnsmasq <-.-> my-pihole-pod-1-pihole

my-pihole-pod-1-custom-dnsmasq <-.-> my-pihole-pod-1-pihole
end
my-pihole-pod-1:::UPDATED
end
my-pihole:::UPDATED
my-pihole-dhcp([
Service Type: NodePort
Name: my-pihole-dhcp
PortMapping:
UDP: <span style='color: red;'>Dynamic 30000-32767,67:client-udp
</span>, <span style='color: green;'>Dynamic 30000-32767,68:client-udp
</span>
]):::UPDATED
my-pihole-dhcp --http: 80
dns: 53
dns-udp: 53
https: 443
client-udp: 67--> my-pihole-pod-0-proxy
my-pihole-dhcp --http: 80
dns: 53
dns-udp: 53
https: 443
client-udp: 67--> my-pihole-pod-1-proxy
my-pihole-dns-udp([
Service Type: NodePort
Name: my-pihole-dns-udp
PortMapping:
UDP: Dynamic 30000-32767,53:dns-udp

<span style='color: green;'>TCP: Dynamic 30000-32767,54:54

</span>]):::UPDATED
my-pihole-dns-udp --http: 80
dns: 53
dns-udp: 53
https: 443
client-udp: 67--> my-pihole-pod-0-proxy
my-pihole-dns-udp --http: 80
dns: 53
dns-udp: 53
https: 443
client-udp: 67--> my-pihole-pod-1-proxy
my-pihole-web([
Service Type: ClusterIP
Name: my-pihole-web
PortMapping:
TCP: <span style='color: red;'>80:http
</span>, <span style='color: red;'>443:https
</span>, <span style='color: green;'>8080:http
</span>
<span style='color: green;'>UDP: 53:dns-udp

</span>]):::UPDATED
my-pihole-web --http: 80
dns: 53
dns-udp: 53
https: 443
client-udp: 67--> my-pihole-pod-0-proxy
my-pihole-web --http: 80
dns: 53
dns-udp: 53
https: 443
client-udp: 67--> my-pihole-pod-1-proxy
my-pihole-pod-0-proxy --UDP-68 --> my-pihole-dhcp
my-pihole-pod-1-proxy --UDP-68 --> my-pihole-dhcp
my-pihole-smoke-test-proxy --UDP-68 --> my-pihole-dhcp
my-pihole-pod-0-proxy --UDP-53 TCP-54 --> my-pihole-dns-udp
my-pihole-pod-1-proxy --UDP-53 TCP-54 --> my-pihole-dns-udp
my-pihole-smoke-test-proxy --UDP-53 TCP-54 --> my-pihole-dns-udp
my-pihole-pod-0-proxy --TCP-8080 UDP-53 --> my-pihole-web
my-pihole-pod-1-proxy --TCP-8080 UDP-53 --> my-pihole-web
my-pihole-smoke-test-proxy --TCP-8080 UDP-53 --> my-pihole-web
classDef DELETED stroke:#f00
classDef NEW stroke:#0f0
classDef UPDATED stroke:#00f```