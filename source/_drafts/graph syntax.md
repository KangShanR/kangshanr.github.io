# markdown graph syntax

[mermaid live editor](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZ3JhcGggVERcbiAgICBBW0NocmlzdG1hc10gLS0-fEdldCBtb25leXwgQihHbyBzaG9wcGluZylcbiAgICBCIC0tPiBDe0xldCBtZSB0aGlua31cbiAgICBDIC0tPnxPbmV8IERbTGFwdG9wXVxuICAgIEMgLS0-fFR3b3wgRVtpUGhvbmVdXG4gICAgQyAtLT58VGhyZWV8IEZbZmE6ZmEtY2FyIENhcl0iLCJtZXJtYWlkIjp7InRoZW1lIjoiZGVmYXVsdCJ9LCJ1cGRhdGVFZGl0b3IiOmZhbHNlfQ)

[mermaid syntax](https://mermaid-js.github.io/mermaid/#/)

## Example

```mermaid
graph TD
    I([场站是否有距离配置])
    H([开启充电成功])
    O([充电坐标数据收集])
    L(不在配置范围)
    G[fa:fa-car 进入其他检查流程]
    P[设备ID错误]
    A(用户请求充电) --> C[是否指定套餐类型充电]
    C ==>|yes| D([检查设备ID是否正确])
    D -->|no| P
    P -->M
    D ==>|yes| I
    I ==>|yes| J([是否在配置范围内])
    J -->|no|L
    J ==>|yes| G
    I -->|no| G
    L -->|定位问题请求失败|M(请求结束)
    L -->O
    C -->|no| G
    G ==>|检查通过|H
    H ==>O & M
```
