# Angular \(2+ / 4+\) - Model \(Class\) or Interface?

The **Interface** describes either a **contract for a class** or a new type. It is a pure Typescript element, so it doesn't affect Javascript. A **model**, and namely a **class**, is an actual JS function which is being used to generate new objects.

#### Model:

While TypeScript has [interfaces](https://blogs.msdn.microsoft.com/typescript/2013/01/24/walkthrough-interfaces/) that can provide this functionality, the Angular team recommends \([Agular Style Guide](https://angular.io/styleguide#!#03-03)\) just using a bare ES6 class with strongly typed instance variables. ES6 classes allow you to \(optionally\) build out functionality around your models and also doesn't require you to be locked into a TypeScript specific feature. For these reasons, it's advisable to use classes for creating models.

A simple example of this is a User class that defines a name variable that's a string and an age variable that must be a number:

long version **with constructor method** \(providing default values\):

```
export class Highscore {

  constructor(
    public namePlayer: string = '',
    public scorePlayer: number = 0,
    public scoreComputer: number = 0,
    public score: number = 0
  ) {}

}

```

...same with short version **without constructor method** \(providing default values\):

```
export class Highscore {
  namePlayer = '';
  scorePlayer = 0;
  scoreComputer = 0;
  score = 0;
}
```

Per the Angular Style Guide, models should be stored under a shared/ folder if they will be used in more than one part of your application \(which models typically are\).



#### Interface:

```
export interface IHighscores {
  name: string;
  score: number;
}
```



#### From the Angular Style Guide:

> ### Interfaces {#-a-id-03-03-a-interfaces}
>
> #### [STYLE 03-03](https://angular.io/styleguide#03-03) {#-a-href-03-03-style-03-03-a-}
>
> **Do**name an interface using upper camel case.
>
> **Consider**naming an interface without an`I`prefix.
>
> **Consider**using a class instead of an interface.
>
> **Why?**[TypeScript guidelines](https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines)discourage the`I`prefix.
>
> **Why?**A class alone is less code than a_class-plus-interface_.
>
> **Why?**A class can act as an interface \(use`implements`instead of`extends`\).
>
> **Why?**An interface-class can be a provider lookup token in Angular dependency injection.



