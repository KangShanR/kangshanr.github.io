# markdown graph syntax

[mermaid live editor](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZ3JhcGggVERcbiAgICBBW0NocmlzdG1hc10gLS0-fEdldCBtb25leXwgQihHbyBzaG9wcGluZylcbiAgICBCIC0tPiBDe0xldCBtZSB0aGlua31cbiAgICBDIC0tPnxPbmV8IERbTGFwdG9wXVxuICAgIEMgLS0-fFR3b3wgRVtpUGhvbmVdXG4gICAgQyAtLT58VGhyZWV8IEZbZmE6ZmEtY2FyIENhcl0iLCJtZXJtYWlkIjp7InRoZW1lIjoiZGVmYXVsdCJ9LCJ1cGRhdGVFZGl0b3IiOmZhbHNlfQ)

[mermaid syntax](https://mermaid-js.github.io/mermaid/#/)

## Example

```mermaid
graph TD
    P((P))
    N((N))
    S((S))
    Sl((Sl))
    Sr((Sr))
    
    
    P-.->N
    S-->P
    P-->Sl
    S-->Sr
    

    class Sl red;
    class P,S,Sr black;
    class N depr;
    classDef black color:#fff,fill:#111,stroke:#999,stroke-width:1px;
    classDef red color:#fff,fill:#f11,stroke:#999,stroke-width:1px;
    classDef depr color:#eee,fill:#555,stroke:#999,stroke-width:1px;
```
