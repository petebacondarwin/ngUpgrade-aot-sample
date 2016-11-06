# Sample for ngUpgrade with AOT

## Overview

![Overview](overview.png)

## Structure

- ``app/app.module.ts``: Angular 1 module
- ``app/app2.module.ts``: Angular 2 module
- ``app/app.ts``: Bootstraps the hybrid application

## Problems

The FlightSearchComponent is not able to display the upgraded FlightCardComponent (after searching for flights from ``Graz`` to ``Hamburg``). 

The FlightCardComponent is registered within the file ``app.module.ts`` which creates an angular1 module. The file ``app2.module.ts`` defines an ng2-based facade for it.
 
The same holds true for a component ng1 that works like the ng1-component in the test cases.

## The good message

Every other scenario displayed above already works in this sample.

## Build and Start

- Build: ``npm run build`
    - Runs ngc and webpack
- Start: ``npm start``
    - Does not created a build, so run ``npm run build`` before
    - Starts live-server
    
## Other questions

1. Why don't we have a convenience function for upgrading, like we had when using UpgradeAdapter? For instance, currently we have to do this, to perform an upgrade:

    ```
    @Directive({selector: 'ng1'})
    export class Ng1 extends UpgradeComponent {
    
        constructor(elementRef: ElementRef, injector: Injector) {
            super('ng1', elementRef, injector);
        }
    }
    ```
    
    Is this because a convenience function would be too dynamic for the compiler?
    
    ** @petebd: yes, the AoT compiler needs to have the directive information statically available at compile time.
       and it is not clever enough to parse such a helper function outside of running it - even if it is a pure function.
    **

2. Why do all the samples and test cases use ``platformBrowserDynamic``, although we are using AOT? 

    ```
    bootstrap(
        platformBrowserDynamic(),
        AppModuleNgFactory,
        document.body,
        'flight-app')
    ```
    ** @petebd: I agree that this is not ideal but the unit testing setup doesn't make it easy to run the AoT
       compiler on the example code at this time. So we focussed on testing the code using the dynamic compiler
       for now, since the only difference will be if we are relying upon something that is removed by the AoT
       compiler. We will use e2e tests to test that AoT actually works.
    **
    
