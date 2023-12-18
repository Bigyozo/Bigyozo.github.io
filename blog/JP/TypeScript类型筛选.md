# TypeScript 型抽出

Extract を使用することで、特定の文字列で構成されるセットを取得できる

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
        //この場合、data[key] を使用してもエラーは発生しない
        if(data[key]){
        }
    }
}
```
