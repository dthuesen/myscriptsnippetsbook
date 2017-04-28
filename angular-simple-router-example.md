# Angular \(2+, 4+\) - simple Router example.

```
import { Routes, RouterModule } from '@angular/router';

import { GameComponent } from './game/game.component';
import { HomeComponent } from './home/home.component';

export const appRoutes: Routes = [
  {path: '', component: HomeComponent},
  {path: 'game', component: GameComponent},
  {path: '**', component: HomeComponent},
];

export const appRouting = RouterModule.forRoot(appRoutes);
export const routingComponents = [
  GameComponent,
  HomeComponent
];
```





