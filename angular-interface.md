# Angular \(2+ / 4+\) - Model \(Class\) or Interface?

The **Interface** describes either a **contract for a class** or a new type. It is a pure Typescript element, so it doesn't affect Javascript. A **model**, and namely a **class**, is an actual JS function which is being used to generate new objects.

```
export interface IHighscores {
  name: string;
  score: number;
}
```



