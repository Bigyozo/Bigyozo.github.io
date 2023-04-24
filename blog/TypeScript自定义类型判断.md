# TypeScript 自定义类型判断

使用 typeof 和 instanceof 可以对 js 的原生类型进行判断，但对于自定义的类型进行判断则需要使用自定义类型守卫。

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
