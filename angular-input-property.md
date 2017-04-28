# Angular \(2+, 4+\) - @Input property

The Input is for passing data from a container \(or parent\) component to a child component. The Input property will take the data from the view down into its controller. Therefore the Input property is placed in the controller of the ChildComponent Two components: **ContainerComponent** + **ChildComponent**

View file **ContainerComponent.htm** looks like so:

```
<app-child [displayText]="computerText"></app-child>
```

Controller file **ChildComponent.ts **than looks like so:

```
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {

  /** 
   * Here the input has an alias. The property of from view is named 'displayText' 
   * but in the component itself it will be used as 'this.computerDisplayText'
   */
  @Input('displayText') computerDisplayText: string;

  constructor() { }

}
```

With that said, the view of the child component, the **ChildComponent.html,** would use the property like so:

```
<h1>{{computerDisplayText}}</h1>
```

---

# Angular \(2+, 4+\) - @Output property

Passing data from a child component to a container \(parent\) component uses the EventEmitter provided by @angular/core.

As the name says it emits data triggered by an event. Therefore the event will be e.g. triggered by an event from the view of the child component. this could be a click event, a mouse over event or a page load event. Or whatever triggers an event. Taking the event from a DOM element, a click event, it could go like this: 

1. The user clicks on a button in the child view, 
2. that button calls a method in the component controller, 
3. that method emit an event, 
4. that event will be passed to the Output property, 
5. in the view of the container component an event binding will pass the data to a property in the container component controller \(using the $event object\) or to another method which then does something.

In code it looks like this:

The **ChildComponent.html **\(view\):

```
<button (click)="setPlayersChoice('scissors')">Schere</button>
```

The **ChildComponent.ts** \(controller\):

```
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {

  // The Output property emits to the container component
  @Output() choiceSet = new EventEmitter();

  constructor() {}
  
  setPlayersChoice(choice) {
    this.choiceSet.emit(choice);
  }
}
```

 The ContainerComponent.html \(view\):

```
<app-child (choiceSet)="setPlayersChoice($event)" ></app-child>
```

The ContainerComponent.ts \(controller\):

```
import { Component } from '@angular/core';
import { PlayerComponent } from '../player/player.component';

@Component({
  selector: 'app-container',
  templateUrl: './container.component.html',
  styleUrls: ['./container.component.css']
})
export class GameComponent {

    playersChoice = ''; // Type string trivially inferred from a string literal, no type annotation needed 

    constructor() {}
  
    setPlayersChoice(choice) {
      this.playersChoice = choice;
    }
  }  
}
```



