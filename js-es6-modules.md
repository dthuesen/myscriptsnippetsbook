# JavaScript - ES6 Modules

Setting up ES6 Modules can be a pain in the a\*\*\*\*\*\*\*\*\*\*\* :-\)

**Prequesites**:

* Install in an temporary directory the npm package '**babel-polyfill**' and copy from its module directory the file '**polyfill.js**' into a folder named 'loaders' or '**assets**'
* Copy the SystemJS code from the URL [**https://jspm.io/system@0.19.34.js**](https://jspm.io/system@0.19.34.js) into a file called '**system.js**' and place that file in the same folder like the '**polyfill.js**' file \(e.g. assets\)

* Embed the the link to these files above into the html file named index.html in the **head section**, like so:  
  `<script src="loaders/system.js"></script>   
   <script src="loaders/polyfill.js"></script>`

* Create a file '**config.js**' and place it in the assets folder , too. In this **config.js** file add the following configuration:

```
/*global system*/
'use strict';


System.config({ 
  transpiler: 'babel',
  packages: {
    './': {
      defaultExtension: false
    }
  },
  map: {
  },
})
```

* Now embed the following script into the body tag:

```
<script>
    console.log('Loading modules...');     
    System.import('./index.js');              // <--- the starting file for the modules 
</script>                                     //      (it can be named as you like e.g start.js or whatever)
```

* In the starting file \(e.g. index.js or start.js or app.js\) the composition of modules can begin...

#### ES6 Modules

**In index.js:**

```
import { Module1 } from './module1.js';         // <-- import the module

module1 = new Module1();                        // <-- instantiate the module

module1.functionXyz();                          // <-- call a method from the module

console.log(module1.propertyXyz);               // <-- use a property of the module
```

In the module module1.js:

```
export class Module1 {
    constructor() {
        this.propertyYxz = 'A string in spring.'
        this.property2 = 'Log this out from this module.'
    }
    
    functionXyz() {
        console.log('This method is only named functionXyz but as a function of an Object it is actually a method!')
        console.log(this.property2)
        this.method2();
    }
    
    method2() {
        console.log('log this out in this class...')
    }
}
```











