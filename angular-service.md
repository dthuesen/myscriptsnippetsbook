# Angular \(2+ / 4+\) - Service

```
import { Injectable } from '@angular/core';
import { IHighscores } from './highscores';

@Injectable()
export class HighscoresService {
  public highscores: IHighscores[];     // The interface has to be declares either private or public!

  constructor() {}

}
```



