# Angular Testing - TDD test setup

* [1. Import all related Modules, Components, Services, etc upfront](#1-import-all-related-modules-components-services-etc-upfront)
* [2. Basic structure](#2-basic-structure)
* [3. Testing the view](#3-testing-the-view)

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
import { MdCard } from '@angular/material';

describe('Rock, Paper, Stone Game - GameComponent (container component)', () => {

  beforeEach( () => {                                // Setups for all suites and specs
    TestBed.configureTestingModule({                 // Configuartion of testbed...
      declarations: [                                // ...with all declarations...
        ComputerComponent,
        GameComponent,
        MdCard
      ],
      imports: [                                     // ...and related modules
        FormsModule,
        BrowserAnimationsModule,
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













