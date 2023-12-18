# TypeScript 自作型のタイプ判定

typeof と instanceof は JavaScript のネイティブな型しか判別できないため、カスタム型に対しては Type Guard を使用する必要がある

```typescript
export interface AgentUser {
  agentInfo: string;
}
export interface StaffUser {
  staffInfo: string;
}
export type User = AgentUser | StaffUser;
```

```typescript
isAgentUser(res: User): res is AgentUser{
    return 'agentInfo' in res;
}
isStaffUser(res: User): res is StaffUser{
    return 'staffInfo' in res;
}

getUserInfo(res: User){
    if(this.isAgentUser(res)){
        return res.agentInfo;
    }else if(this.isStaffUser(res)){
        return res.staffInfo;
    }else{
        return null;
    }
}

```
