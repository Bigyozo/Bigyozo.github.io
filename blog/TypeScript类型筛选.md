# TypeScript 类型筛选

使用 Extract 可以对类型的 key 进行筛选，得到由特定的 key 构成的集合

```typescript
export interface Data {
  userName: string;
  userName_new: string;
  address: string;
  address_new: string;
  phoneNumber: string;
  phoneNumber_new: string;
}
//NewItemsInData = 'userName_new' | 'address_new' | 'phoneNumber_new'
export type NewItemsInData = Extract<keyof Data, `${string}_new`>;
```

```typescript
foo(data:Data){
    const keys : NewItemsInData[] = ['userName_new','address_new','phoneNumber_new'];
    for(let key of keys){
        //此时使用data[key]不会报错
        if(data[key]){
        }
    }
}
```
