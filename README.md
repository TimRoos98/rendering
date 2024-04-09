```mermaid
graph LR
    A[Block A] --> B[Block B]
    B --> C[Block C]
    C --> D[Block D]
    D --> E[Block E]
    subgraph Top Layer
        A --> B
    end
    subgraph Middle Layer
        B --> C
    end
    subgraph Bottom Layer
        C --> D
        D --> E
    end
    A:::DELETED
classDef DELETED stroke:#f00
classDef NEW stroke:#0f0
classDef UPDATED stroke:#00f

```
