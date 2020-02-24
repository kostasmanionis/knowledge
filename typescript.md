[Using Typescript to make invalid states irrepresentable](http://www.javiercasas.com/articles/typescript-impossible-states-irrepresentable)

```ts
type NoFields = {
  [key: string]: never;
};

interface Field1Only {
  field1: string;
}

interface BothField1AndField2 {
  field1: string;
  field2: string;
}

type Fields = NoFields | Field1Only | BothField1AndField2;

const broken = { field2: "asdf" };

// Bypass1: go through an empty object type
// Empty object is a well known code smell in Typescript
const bypass1: {} = broken;
const brokenThroughBypass1: Fields = bypass1;

// Bypass2: use the `any` escape hatch
// any is another well known code smell in Typescript
const bypass2: any = broken;
const brokenThroughBypass2: Fields = bypass2;
```
