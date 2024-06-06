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
        Parallelogram[/ Secret /]
        ParallelogramAlt[\ ConfigMap \]
    end
    subgraph Deployment
        direction LR
        subgraph rootConfig[Deployment Specific Config]
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
    end
my-pihole-smoke-test-proxy --> my-pihole-web-service
subgraph my-pihole-smoke-test [Pod: my-pihole-smoke-test
]
direction BT
subgraph config-my-pihole-smoke-test [CONFIG:
name: my-pihole-smoke-test
]
end
my-pihole-smoke-test-hook1-container(
hook1-container
Resources: &#40request/limits&#41
cpu: Default/Default
memory: Default/Default)
my-pihole-smoke-test-proxy{"Ingress
Egress
"}
my-pihole-smoke-test-hook1-container --> my-pihole-smoke-test-proxy
end
subgraph my-pihole [Deployment: my-pihole]
subgraph my-pihole-config [Config:
name: my-pihole
]
end
subgraph my-pihole-pod-0 [Pod: my-pihole-pod-0
]
direction BT
subgraph config-my-pihole-pod-0 [CONFIG:
name: pod-0
]
end
my-pihole-pod-0-pihole(
pihole
Resources: &#40request/limits&#41
cpu: Default/Default
memory: Default/Default)
my-pihole-pod-0-proxy{"Ingress
Egress
"}
my-pihole-pod-0-pihole <-->my-pihole-pod-0-proxy
my-pihole-pod-0-config[("config
type: emptyDir
")]
my-pihole-pod-0-config2[("config2
type: emptyDir
")]
my-pihole-pod-0-custom-dnsmasq[("custom-dnsmasq
type: configMap
defaultMode: 420
name: my-pihole-custom-dnsmasq")]
my-pihole-pod-0-config <-. "/etc/pihole" .-> my-pihole-pod-0-pihole

my-pihole-pod-0-custom-dnsmasq <-. "/etc/dnsmasq.d/02-custom.conf" .-> my-pihole-pod-0-pihole

my-pihole-pod-0-custom-dnsmasq <-. "/etc/addn-hosts" .-> my-pihole-pod-0-pihole
end
subgraph my-pihole-pod-n [Pod: my-pihole-pod-n
]
direction BT
subgraph config-my-pihole-pod-n [CONFIG:
name: pod-n
]
end
my-pihole-pod-n-pihole(
pihole
Resources: &#40request/limits&#41
cpu: Default/Default
memory: Default/Default)
my-pihole-pod-n-proxy{"Ingress
Egress
"}
my-pihole-pod-n-pihole <-->my-pihole-pod-n-proxy
my-pihole-pod-n-config[("config
type: emptyDir
")]
my-pihole-pod-n-config2[("config2
type: emptyDir
")]
my-pihole-pod-n-custom-dnsmasq[("custom-dnsmasq
type: configMap
defaultMode: 420
name: my-pihole-custom-dnsmasq")]
my-pihole-pod-n-config <-. "/etc/pihole" .-> my-pihole-pod-n-pihole

my-pihole-pod-n-custom-dnsmasq <-. "/etc/dnsmasq.d/02-custom.conf" .-> my-pihole-pod-n-pihole

my-pihole-pod-n-custom-dnsmasq <-. "/etc/addn-hosts" .-> my-pihole-pod-n-pihole
end
subgraph my-pihole-pod-4 [Pod: my-pihole-pod-4
]
direction BT
subgraph config-my-pihole-pod-4 [CONFIG:
name: pod-4
]
end
my-pihole-pod-4-pihole(
pihole
Resources: &#40request/limits&#41
cpu: Default/Default
memory: Default/Default)
my-pihole-pod-4-proxy{"Ingress
Egress
"}
my-pihole-pod-4-pihole <-->my-pihole-pod-4-proxy
my-pihole-pod-4-config[("config
type: emptyDir
")]
my-pihole-pod-4-config2[("config2
type: emptyDir
")]
my-pihole-pod-4-custom-dnsmasq[("custom-dnsmasq
type: configMap
defaultMode: 420
name: my-pihole-custom-dnsmasq")]
my-pihole-pod-4-config <-. "/etc/pihole" .-> my-pihole-pod-4-pihole

my-pihole-pod-4-custom-dnsmasq <-. "/etc/dnsmasq.d/02-custom.conf" .-> my-pihole-pod-4-pihole

my-pihole-pod-4-custom-dnsmasq <-. "/etc/addn-hosts" .-> my-pihole-pod-4-pihole
end
end
my-pihole-web-service([
Service Type: ClusterIP
name: my-pihole-web
])
my-pihole-web-service <-->my-pihole-pod-0-proxy
my-pihole-web-service <-->my-pihole-pod-n-proxy
my-pihole-web-service <-->my-pihole-pod-4-proxy
subgraph KubernetesAPI [KubernetesAPI
]
my-pihole-password[/
Secret
name: my-pihole-password
/]
my-pihole-custom-dnsmasq[\
ConfigMap
name: my-pihole-custom-dnsmasq
\]
my-pihole-dnsmasq2[\
ConfigMap
name: my-pihole-dnsmasq2
\]
my-pihole-configmap-ns[\
ConfigMap
name: my-pihole-configmap-ns
\]
end

```