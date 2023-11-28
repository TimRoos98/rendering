```mermaid
flowchart TD
    subgraph Deployment-x
        direction RL
        G{PortMap}
        scalar((1..3))
        G--tcp 80-->Container-1("`Name: NXING-POD
                        Image: name/docker-image:version`")
        G--tcp 443-->Container-2("`Name: NXING-POD
                        Image: name/docker-image:version`")
        G--udp 161>Container-3("`Name: NXING-POD
                        Image: name/docker-image:version`")
    end

```
```mermaid
flowchart TD
    user1[fa:fa-user User 1] -- edit --> folder
```
```mermaid
graph LR
    user1[fa:fa-user User 1] -- edit --> folder
```
