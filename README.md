```mermaid
flowchart TD
    subgraph Deployment-x
        direction RL
        G{PortMap}
        scalar((1..3))
        G--https 443\n fa:fa-globe 80-->Container-1("`Name: NXING-POD
                        Image: name/docker-image:version`")
        G-->Container-2("`Name: NXING-POD
                        Image: name/docker-image:version`")
        G-->Container-3("`Name: NXING-POD
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
