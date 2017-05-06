# Angular Testing - TDD test setup

* [1. Import all related Modules, Components, Services, etc upfront](#1-import-all-related-modules-components-services-etc-upfront)
* [2. Basic structure](#2-basic-structure)
* [3. Testing the view](#3-testing-the-view)
* [4. Testing the components properties](#4-testing-the-components-properties)

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

a\) Testing a specific `tag` an its `textContent`

b\) Testing if a specific `tag` is present

c\) Testing if a `button` queried by `tag` and `id` is labeled with 'Reset'

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

});
```

#### 4. Testing the components properties

a\) Test for `property` and its declared value

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

});
```







