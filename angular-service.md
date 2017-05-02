# Angular \(2+ / 4+\) - Service and DI

#### Global provision of a service \(most common\):

The service will be provided in the main module \(e.g. app module\) and is then available for all components belonging to this module and it can also be imported into other modules via 'import'.

The service then has to be imported in the relevant component.

```js
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
import { NgModule } from '@angular/core';
import { routingComponents, appRouting } from './app.routing';

import { AppComponent } from './app.component';
import { HighScoreComponent } from './high-score/high-score.component';

// Services
import { HighscoresService } from './highscores.service';           // Import the service


@NgModule({
  declarations: [
    AppComponent,
    routingComponents,
    HighScoreComponent,
  ],
  imports: [
    appRouting,
    BrowserModule,
    FormsModule,
    HttpModule,
  ],
  providers: [HighscoresService],                                   // Provide the service
  bootstrap: [AppComponent]
})
export class AppModule { }}
```

#### Local provision of a service \(only for one component\):

Instead of making a service globally available it is also possible to provide it only in a component itself. But this is only useful if the service belongs only to one component. If this service is injected in several components changes to it would make a mess, probably.

```js
import { Component, Inject } from '@angular/core';            // import 'Inject' for injection of the service
import { HighscoresService } from '../highscores.service';    // import the service
import { IHighscore } from '../highscore';                    // import the interface

@Component({
  selector: 'app-game',
  templateUrl: './game.component.html',
  styleUrls: ['./game.component.css'],
})
export class GameComponent {

  title = '.....';
  // ....
  // ...

  constructor(@Inject(HighscoresService)                    // injection with @Inject decorator in the constructor 
      private highscoresService: HighscoresService
    ) {
      this.highscores = highscoresService.highscores;
    }
```

#### Definition of a service

```js
import { Injectable } from '@angular/core';
import { IHighscores } from './highscores';

@Injectable()                           // The @Injectable decorator makes the service injectable to components
export class HighscoresService {
  public highscores: IHighscores[];     // The interface has to be declared either private or public!

  constructor() {}

}
```

#### Example of a service

The HighscoreService is responsible for providing a list of score and adding new entries to it. 

1. This imports the **`Highscore`** model
2. The **`highscores`** property is the data Array for all entries an starts with an empty array
3. **`scoreCache`** is for storing the data of the just ended game. It will be filled with an object of type Highscore \(model\)
4. **`pushHighscore`** will be called by a component with an data object type **`Highscore`**.
5. First **`pushHighscore`** calls calculate score to get a conceived kind of score indicator and then it set the current scores to the **`scoreCache`** object. 
6. After the **`scoreCache`** is set from within **`pushHighscore`** to current scores, name and the score indicator, it pushes the data to the **`Highscores`** Array
7. Finally **`pushHighscore()`** causes to empty the **`scoreCache`** object \(or better to fill it with default values, wich will be overridden if there are any new values from the next game\).

```
import { Injectable } from '@angular/core';
import { Highscore } from './shared/highscore.model';       // Import model

@Injectable()
export class HighscoresService {
  // Provide an empty array to push into
  highscores = [];
  // Construct an object with default values (from model)
  scoreCache = new Highscore();

  constructor() {}

  pushHighscore(score: Highscore) {
    this.calculateScore(score);
    console.log('score object: ', score);
    this.scoreCache.namePlayer    = score.namePlayer;
    this.scoreCache.scorePlayer   = score.scorePlayer;
    this.scoreCache.scoreComputer = score.scoreComputer;

    this.highscores.push(this.scoreCache);

    // Instantiate a new default object otherwise the
    // existing one will be taken and its value only overwritten!!
    this.scoreCache = new Highscore();
  }

  calculateScore (score: Highscore) {
    const player = score.scorePlayer;
    const computer = score.scoreComputer;
    this.scoreCache.score = (player - computer);
  }

}
```



