# Angular Testing - TDD test setup and examples

* [1. Import all related Modules, Components, Services, etc upfront](#1-import-all-related-modules-components-services-etc-upfront)
* [2. Basic structure](#2-basic-structure)
* [3. Testing the view](#3-testing-the-view)
* [4. Testing the components properties](#4-testing-the-components-properties)
* [5. Testing the components methods](#5-testing-the-components-methods)
* [6. How to get elements, properties, etc...](#6-how-to-get-elements-properties-etc)
* [7. Improve performance for test cycles:](#7-improve-performance-for-test-cycles)

#### 1. Import all related Modules, Components, Services, etc upfront

First, it is good to keep in mind, that every component, module, service or directive, that is dedicated to the component which is under test, has to be imported. That means, that if e.g. the view should be tested and Angualr Material components are included, they have to be imported to the test suite, like the component under test itself.

#### 2. Basic structure

```
import { async, fakeAsync, tick, ComponentFixture, TestBed } from '@angular/core/testing';
import { By } from '@angular/platform-browser';

import { BrowserAnimationsModule } from '@angular/platform-browser/animations'
import { FormsModule } from '@angular/forms';

import { ComputerComponent } from '../computer/computer.component';
import { GameComponent } from './game.component';
import { HighscoresService } from '../highscores.service';
import { MdCard } from '@angular/material';

describe('Rock, Paper, Stone Game - GameComponent (container component)', () => {

  beforeEach( () => {                                // Setups for all suites and specs
    TestBed.configureTestingModule({                 // Configuartion of testbed...
      declarations: [                                // ... with all declarations...
        ComputerComponent,
        GameComponent,
        MdCard
      ],
      imports: [                                     // ... related modules...
        FormsModule,
        BrowserAnimationsModule,
      ],
      providers: [
        HighscoresService                            // ... and services
      ]
    })
    .compileComponents();
  });

  describe('/ Description of a test suite', () => {         // The first test suite
    let component: GameComponent;
    let fixture: ComponentFixture<GameComponent>;

    beforeEach(() => {                                      // Setups for each spec in this 
      fixture = TestBed.createComponent(GameComponent);     // specific test suite
      component = fixture.componentInstance;
      fixture.detectChanges();
    });

    it('should create', () => {                             // The initial test spec 
      expect(component).toBeTruthy();
    });

    describe('/ Gamelogic timing tests - properties', () => {    // A further nested test suite...
      let app;

      beforeEach( () => {                                        // ...and setups only for this suite
        app = fixture.debugElement.componentInstance;   
        jasmine.clock().install();                               // Install clock instead of timeout
      });

      afterEach( () => {                                         // Reset to basics
        jasmine.clock().uninstall();                             // Uninstall clock to clear the thimer
      });

      it('Before 0.8s the property "computerText" should be empty',  () => {
        app.countdown();
        jasmine.clock().tick(795);
        expect(app.computerText).toEqual('' || 'Computer is waiting...');
      }); 

    });

  });

});
```

#### 3. Testing the view

a\) Test a specific `tag` an its `textContent`

b\) Test if a specific `tag` is present

c\) Test if a `button` queried by `tag` and `id` is labeled with 'Reset'

d\) Test an `attribute` of a `tag` in this case: the name attribute like here: `<input name="name">`

e\) + f\) When there are more than one `button` in this component, it is easier to test if they have id's for querying them.

```
describe('/ The ListComponent view', () => {

  // a)  
  it('should render title in a h1 tag', () => {
    const compiled = fixture.debugElement.nativeElement;
    expect(compiled.querySelector('h1').textContent).toContain('This is the hadline');
  });

  // b)
  it('should be able to render PlayerComponent tag (<app-player>)', () => {
    const compiled = fixture.debugElement.query(By.css('app-player'));
    expect(compiled).not.toBe(null);
  });

  // c)
  it('should render button "Neustart"', () => {
    const compiled = fixture.debugElement.nativeElement;
    expect(compiled.querySelector('button#new-game').textContent).toContain('Reset');
  });

  // d)
  it('should render a <input> tag with attribute name="name"', () => {
    const compiled = fixture.debugElement.query(By.css('input'));
    expect(compiled).not.toBe(null);
    expect(compiled.attributes['name']).toBe('name');
  });

  // e)
  it('should render a <button> tag with id="save"', () => {
    const compiled = fixture.debugElement.query(By.css('#save'));
    expect(compiled).not.toBe(null);
  });

  // f)
  it('should render a <button> tag with id="abort"', () => {
    const compiled = fixture.debugElement.query(By.css('#abort'));
    fixture.detectChanges();
    expect(compiled).not.toBe(null);
  });

});
```

#### 4. Testing the components properties

a\) Test for `property` and its assigned value

b\) Doubled test for a `property`  -  one for truthiness is present \(I don't think this test is perfect\) and one for **not to be** `undefined` \(has more value for the result\)

    describe('/ ListComponent - properties', () => {

      // a)
      it(`should have a property "title": 'The list of games'`, () => {
        const app = fixture.debugElement.componentInstance;
        expect(app.title).toEqual('The list of games');
      });

      // b)
      it('should have a property "playersScore"', () => {
        const app = fixture.debugElement.componentInstance;
        expect(app.playersScore).toBeTruthy;
        expect(app.playersScore).not.toBe(undefined);
      });

    });

#### 5. Testing the components methods

a\) The basic test for a method is like testing a property's presence

b\) As well a basic test but with manual starting the Change Detection of Angular

c\) Testing a method with several linked properties. In this case the method sets these properties to an initial value. To test it, these properties are declared as local variables of the specific test spec. Then the method 'newGame\(\)' is called and the spec compares the values.

d\) This spec calls the method with an `argument` and tests if the `property`, which should be assigned with a specific `value` returned by this method, is equal.

e\) This spec calls the method with several different arguments and tests if the expected value will be returned.

f\) Testing a method which returns a random value is nearly useless but one can test if the minimum and maximum will not be exceeded.

g\) Test if a function/method has been called. In this case a spy will be set on a component and the method to be tested \(`h spyOn(component, 'calculateWinner');`  the parameters are: component, nameOfTheMethod. The component has to be assigned upfront e.g. `let component: GameComponent;`

h\) Another test with expectation to a set property after a method has been called.

i\) Test whether a `property` is a **function or not **

```
describe('/ 1. Game - methods general', () => {

  // a)
  it('should have a method "newGame()"', () => {
    const app = fixture.debugElement.componentInstance;
    expect(app.newGame).not.toBe(undefined);
  });

  // b)
  it('should have a method "countdown()"', () => {
    fixture.detectChanges();
    const app = fixture.debugElement.componentInstance;
    expect(app.countdown).not.toBe(undefined);
  });

  // c) 
  it('"newGame()" should reset all properties', () => {
    const playersScore      = 0;
    const computersScore    = 0;
    const winnerDisplayText = 'Neues Spiel, neues Glück!';
    const playersChoice     = null;
    const computersChoice   = null;
    const playerText        = 'Spiel deine Hand!';
    const computerText      = 'Computer wartet auf dich...';
    this.restartIsActive    = false;
    this.buttonsDisabled    = false;
    fixture.detectChanges();
    const app = fixture.debugElement.componentInstance;
    app.newGame();
    console.log('app.computerText: ', app.computerText);
    expect(app.playersScore === playersScore).toBe(true);
    expect(app.computersScore === computersScore).toBe(true);
    expect(app.winnerDisplayText === winnerDisplayText).toBe(true);
    expect(app.playersChoice).toBe(null);
    expect(app.computersChoice).toBe(null);
    expect(app.playerText === playerText).toBe(true);
    expect(app.computerText === computerText).toBe(true);
  });

  // d)
  it('"setPlayersChoice(scissors)" should set property "restartIsActive" to true', () => {
    const app = fixture.debugElement.componentInstance;
    app.setPlayersChoice('scissors');
    expect(app.restartIsActive).toBe(true);
  });

  // e)
  it('"lookup()" should return 5 for 4 or 9 for 5', () => {
    fixture.detectChanges();
    const app = fixture.debugElement.componentInstance;
    expect(app.lookup(1)).toBe(1);
    expect(app.lookup(2)).toBe(2);
    expect(app.lookup(3)).toBe(3);
    expect(app.lookup(4)).toBe(5);
    expect(app.lookup(5)).toBe(9);
    expect(app.lookup(42)).toBe(undefined);
  });

  // f) 
  it('"setComputersChoice()" it\'s variable should get a random number', () => {
    const app = fixture.debugElement.componentInstance;
    app.setComputersChoice();
    fixture.detectChanges();
    expect(app.computersChoice).toBeGreaterThan(0);
    expect(app.computersChoice).toBeLessThan(10);
  });

  // g)
  it('"setComputersChoice()" should have called setComputersChoiceText()', () => {
    const app = fixture.debugElement.componentInstance;
    const spy = spyOn(component, 'setComputersChoiceText');
    app.setComputersChoice();
    fixture.detectChanges();
    expect(spy).toHaveBeenCalled();
  });

  // h)
  it('"setComputersChoice()" should have set property buttonsDisabled to false', () => {
    const app = fixture.debugElement.componentInstance;
    const buttonsDisabled = app.buttonsDisabled;
    app.setComputersChoice();
    fixture.detectChanges();
    expect(buttonsDisabled).toBe(false);
  });

  // i )
  it('should have a method emitPlayerData()', () => {
    fixture.detectChanges();
    const app = fixture.debugElement.componentInstance;
    expect(typeof component.emitPlayerData === 'function').toBe(true);
  });

});
```

#### 6. How to get elements, properties, etc...

a\) Getting the compilation of the native elements \(the tags of the view\):

```
const compiled = fixture.debugElement.nativeElement;
```

... and puts them directly into the compiled variable - completing the line above like this:

```
const compiled = fixture.debugElement.nativeElement.querySelector('#buttons');
```

... or then extends the code with on extra line to query a tag:

```
buttonsDiv = compiled.querySelector('#buttons');
```



b\) Getting an element by css selector - tag:

```
const compiled = fixture.debugElement.query(By.css('input'));
```

c\) Getting an element by css selector - id:

```
const compiled = fixture.debugElement.query(By.css('#abort'));
```

d\) Getting an instance of the app:

```
const app = fixture.debugElement.componentInstance;
```

e\) Getting the component:

```
let component: GameComponent;
```

f\) Getting the fixture:

```
let fixture: ComponentFixture<GameComponent>;
```

g\) triggering the change detection of Angular:

```
fixture.detectChanges();
```

f\) Installing jasmine clock \(e.g. in `beforeEach` method\):

```
jasmine.clock().install();
```

h\) Uninstalling the jasmine clock \(e.g. in `afterEach` method\)

```
jasmine.clock().uninstall();
```

i\) Setting the jasmine clock \(e.g. in the specific spec before a function call\)

```
jasmine.clock().tick(795);
```

j\) getting a specific attribute of a tested dom element \(e.g. the name attribute `<input name="name">`\)

```
expect(compiled.attributes['name']).toBe('name');
```

#### 7. Improve performance for test cycles:

The standard call for the test is `ng test` but this leads to a slow pace of the test cycles. turning off the source maps makes it fast as a flash For turning off the source maps call `ng test --sourcemaps=false`

Another way to speed-up the testing is to set a focus to only one spec or suite with using `fdescribe()` or `fit()`. Or with turning off several suites with xdescribe\(\).

The speed issue is under observation and there are already some fixes. Maybe a new version of the cli will get big improvements. For following the issue ticket on github see "[Test development cycle is slow unless sourcemaps are turned off\#5423](https://github.com/angular/angular-cli/issues/5423)".

#### 8. Code coverage

It's always good practice to have an eye on the unit test code coverage. A good cover is nearly or exactly 100 percent.

To find out what coverage the current project actually has just type either

* `ng test --code-coverage=true` 
* or the shorter version: `ng test -cc=true`



