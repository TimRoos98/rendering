```mermaid
flowchart TD
    subgraph Deployment-x
        direction RL
        G{PortMap}
        scalar((1..3))
        G--|fa:fa-globe 80,8080|
        |fa:fa-lock 443|-->Container-1("`Name: NXING-POD
                        Image: name/docker-image:version`")
        G-->Container-2("`Name: NXING-POD
                        Image: name/docker-image:version`")
        G-->Container-3("`Name: NXING-POD
                        Image: name/docker-image:version`")
    end
```
