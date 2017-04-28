# Angular \(2+, 4+\) - @Input property

Two components: ContainerComponent + ChildComponent

View file **ContainerComponent.htm** looks like so:

```
<app-child [displayText]="computerText"></app-child>
```

Controller file **ChildComponent **than looks like so:

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

With that said, the view of the **ChildComponent** would use the property like so:

```
<h1>{{computerDisplayText}}</h1>
```



---

# Angular \(2+, 4+\) - @Output property  



