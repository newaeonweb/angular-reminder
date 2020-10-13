# Angular Reminder

A collection of how to use in Angular framework.


<!-- MarkdownTOC autolink="true" autoanchor="true" -->

- [Introduction](#introduction)
- [Component Lifecycle in order of exection](#component-lifecycle-in-order-of-exection)
  - [ngOnChanges](#ngonchanges)
  - [ngOnInit](#ngoninit)
  - [ngDoCheck](#ngdocheck)
  - [ngAfterContentInit](#ngaftercontentinit)
  - [ngAfterContentChecked](#ngaftercontentchecked)
  - [ngAfterViewInit](#ngafterviewinit)
  - [ngAfterViewChecked](#ngafterviewchecked)
  - [ngOnDestroy](#ngondestroy)
- [Data binding syntax](#data-binding-syntax)
- [Change Detection Strategy](#change-detection-strategy)
  - [Change Detection Default](#change-detection-default)
  - [Change Detection on Push](#change-detection-on-push)

<!-- /MarkdownTOC -->

<a id="introduction"></a>
# Introduction

<a id="component-lifecycle-in-order-of-exection"></a>
# Component Lifecycle in order of exection

<a id="ngonchanges"></a>
## ngOnChanges

ngOnChanges triggers following the modification of @Input bound class members. Data bound by the @Input() decorator come from an external source. When the external source alters that data in a detectable manner, it passes through the @Input property again.

With this update, ngOnChanges immediately fires. It also fires upon initialization of input data. The hook receives one optional parameter of type SimpleChanges. This value contains information on the changed input-bound properties.

<a id="ngoninit"></a>
## ngOnInit

ngOnInit fires once upon initialization of a component’s input-bound (@Input) properties. The next example will look similar to the last one. The hook does not fire as ChildComponent receives the input data. Rather, it fires right after the data renders to the ChildComponent template.

<a id="ngdocheck"></a>
## ngDoCheck

ngDoCheck fires with every change detection cycle. Angular runs change detection frequently. Performing any action will cause it to cycle. ngDoCheck fires with these cycles. Use it with caution. It can create performance issues when implemented incorrectly.

ngDoCheck lets developers check their data manually. They can trigger a new application date conditionally. In conjunction with ChangeDetectorRef, developers can create their own checks for change detection.

<a id="ngaftercontentinit"></a>
## ngAfterContentInit

ngAfterContentInit fires after the component's content DOM initializes (loads for the first time). Waiting on @ContentChild(ren) queries is the hook's primary use-case.

@ContentChild(ren) queries yield element references for the content DOM. As such, they are not available until after the content DOM loads. Hence why ngAfterContentInit and its counterpart ngAfterContentChecked are used.

<a id="ngaftercontentchecked"></a>
## ngAfterContentChecked

ngAfterContentChecked fires after every cycle of change detection targeting the content DOM. This lets developers facilitate how the content DOM reacts to change detection. ngAfterContentCheckedcan fire frequently and cause performance issues if poorly implemented.

ngAfterContentChecked fires during a component’s initialization stages too. It comes right after ngAfterContentInit.

<a id="ngafterviewinit"></a>
## ngAfterViewInit

ngAfterViewInit fires once after the view DOM finishes initializing. The view always loads right after the content. ngAfterViewInit waits on @ViewChild(ren) queries to resolve. These elements are queried from within the same view of the component.

<a id="ngafterviewchecked"></a>
## ngAfterViewChecked

ngAfterViewChecked fires after any change detection cycle targeting the component’s view. The ngAfterViewChecked hook lets developers facilitate how change detection affects the view DOM.

<a id="ngondestroy"></a>
## ngOnDestroy

ngOnDestroy fires upon a component’s removal from the view and subsequent DOM. This hook provides a chance to clean up any loose ends before a component's deletion.

---

<a id="data-binding-syntax"></a>
# Data binding syntax

| Type | Syntax | Category
|--|--|--|
|Interpolation Property Attribute Class Style |{{expression}}  [target]="expression" bind-target="expression" | One-way from data source to view target|
| Event | (target)="statement" on-target="statement"| One-way  from view target  to data source |
| Two-way | [(target)]="expression" bindon-target="expression"  | Two-way |


One Way binding `<p>{{ descri­ption }}<­/p>`

One Way binding `<input [ngMo­del­]=­"­use­r­" />`

Two Way Binding `<input [(ngMo­del­)]=­"­use­r.n­ame­" />`

Property Binding `<img [src]=­"­use­r.i­mag­eUR­L" />`

Attr­ibute Binding `<b­utton [attr.a­ri­a-l­abe­l]=­"­sub­mit­"­>Su­bmi­t</­but­ton­>`

Class Binding `<div [class.se­lec­ted­]="S­ele­cte­d">S­ele­cte­d</­div­>`

ngClass `<div [ngCla­ss]­="{'­sel­ected' : isSele­cte­d()­}">S­ele­ctable Item</­div­>`

ngClass `<div [ngCla­ss]­="ge­tCl­ass­es(­)">I­tem­</d­iv>`

Style Binding `<b­utton [style.co­­lo­r­]­="i­­sSe­­lected ? 'red' : 'white­­'">`

ngStyle `<div [ngSty­le]­="{'­bac­kgr­oun­d-i­mage': 'url(/­pat­h/t­o/i­mg.j­pg­)'}­"­>It­em<­/di­v>`

ngStyle `<div [ngSty­le]­="se­tSt­yle­s()­"> {{customer.name}} </d­iv>`

Comp­onent Binding `<m­y-a­pp-­com­ponent [user]­="ac­tiv­eUs­er">­</m­y-a­pp-­com­pon­ent­>`

Event Binding `<b­utton (click­)="s­ubm­it(­)">S­ubm­it<­/bu­tto­n>`

$event `<input [value­]="n­ame­" (input­)="n­ame­=$e­ven­t.t­arg­et.v­al­ue">`

<a id="change-detection-strategy"></a>
# Change Detection Strategy

Angular propagates changes top-down, from parent components to child components.

Each component in Angular has an equivalent change detection class. As the component model is updated, it compares previous and new values. Then changes to the model are updated in the DOM.

For component inputs like number and string, the variables are immutable. As the value changes and the change detection class flags the change, the DOM is updated.

For object types, the comparison could be one of the following,

Shallow Comparison: Compare object references. If one or more fields in the object are updated, object reference doesn’t change. Shallow check will not notice the change. However, this approach works best for immutable object types. That is, objects cannot be modified. Changes need to be handled by creating a new instance of the object.

Deep Comparison: Iterate through each field on the object and compare with previous value. This identifies change in the mutable object as well. That is, if one of the field on the object is updated, the change is noticed.

<a id="change-detection-default"></a>
## Change Detection Default

Change detection strategy for a component

On a component, we can annotate with one of the two change detection strategies.

ChangeDetectionStrategy.Default:

As the name indicates, it’s the default strategy when nothing is explicitly annotated on a component.

With this strategy, if the component accepts an object type as an input, it performs deep comparison every time there is a change. That is, if one of the fields have changed, it will iterate through all the fields and identify the change. It will then update the DOM.

Consider the following sample:

```
import { Component, OnInit, Input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-child',
  template: '  <h2>{{values.title}}</h2> <h2>{{values.description}}</h2>',
  styleUrls: ['./child.component.css'],
  changeDetection: ChangeDetectionStrategy.Default
})
export class ChildComponent implements OnInit {

// Input is an object type.
  @Input() values: {
    title: string;
    description: string;
  }

  constructor() { }

  ngOnInit() {}

}
```

Notice, change detection strategy is mentioned as ChangeDetectionStrategy.Default, it is the value set by default in any component. Input to the component is an object type with two fields title and description.

Here’s the snippet from parent component template.

`<app-child [values]="values"></app-child>`
An event handler in the parent component is mentioned below. This is triggered, possibly by a user action or other events.

```
updateValues(param1, param2){
    this.values.title = param1;
    this.values.description = param2;
  }
```

Notice, we are updating the values object. The Object reference doesn’t change. However the child component updates DOM as the strategy performs deep comparison.

<a id="change-detection-on-push"></a>
## Change Detection on Push

ChangeDetectionStrategy.OnPush:

The default strategy is effective for identifying changes. However, it’s not performant as it has to loop through a complete object to identify change. For better performance with change detection, consider using OnPush strategy. It performs shallow comparison of input object with the previous object. That is, it compares only the object reference.

The code snippet we just saw will not work with OnPush strategy. In the sample, the reference doesn’t change as we modify values on the object. The Child component will not update the DOM.

When we change strategy to OnPush on a component, the input object needs to be immutable. We should create a new instance of input object, every time there is a change. This changes object reference and hence the comparison identifies the change.

Consider following snippet for handling change event in the parent.

```
updateValues(param1, param2){
  this.values = { // create a new object for each change.
    title: param1,
    description: param2
  }
}
```

Note: Even though the above solution for updateValues event handler might work okay with the OnPush strategy, it’s advisable to use immutable.js implementation for enforcing immutability with objects or observable objects using RxJS or any other observable library.



