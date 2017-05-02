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



