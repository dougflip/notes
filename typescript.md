# Typescript

## Types from Arrays

https://steveholgado.com/typescript-types-from-arrays/

```ts
const animals = ["cat", "dog", "mouse"] as const;
type Animal = typeof animals[number];

// type Animal = 'cat' | 'dog' | 'mouse'
```

```ts
// the keys I want the compiler to know about
export const rtoOfficeResultChart = [
  "officeCases",
  "missedWorkDays",
  "healthcareCosts",
] as const;

// Type that references the above keys
// so we can use it in type signatures
export type RtoOfficeResultChartKey = typeof rtoOfficeResultChart[number];

// an object that uses those keys
export type RtoOfficeResultsData = {
  [K in typeof rtoOfficeResultChart[number]]: {
    dates: string[];
    sim: SimStats;
  };
};

// check any string against the known keys
rtoOfficeResultChart.find((x) => x === unknownString);
```

## Unions vs Enums

Use an object as an enum and extract its values to a type.
Probably not as common as the above, but interesting to see how it can be done!

```ts
// union type equivalent
const Permission = {
  Read: "r",
  Write: "w",
  Execute: "x",
} as const;

type Permission = typeof Permission[keyof typeof Permission]; // 'r' | 'w' | 'x'
```
