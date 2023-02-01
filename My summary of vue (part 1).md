My summary of vue (part 1)

1. Fundamentals of Vue

When a Vue instance is created, Vue iterates over the properties in the data, converts them to getters/setters with Object.defineProperties(Vue 3.0 uses proxy), and tracks dependencies internally, notifying changes when properties are accessed and modified. Each component instance has a corresponding watcher instance, which records the properties as dependencies during component rendering, and then notifies the watcher to recaculate when the dependency's setter is called, causing its associated component to be updated.



2. Priciples of Vue Responsiveness

2.1 What is data hijacking

When we use Object.defineProperty hijacking object accessor,  if the property value changes, then we can get changes, so as to carry out further opreations.



2.2 Publisher Model/Subscriber Model

In software architecture,  publish-subscribe is a messaging paradigm in which the sender of message(called a publisher) does not send the message directly to a specific recipient(called a subscriber). Instead, published messages are grouped into categories without knowing which subscribers (if any) may exist. Likewise, subscribers can express interest in one or more categories and receive only messages of intereset without knowing which publishers (if any) exist.

The difference is that, unlike the observer and the observed, the publisher and the subscriber do not know each other's existence, the publisher only needs to send the message to the subscriber, and the subscriber juset accepets the content the needs to subscribe to .



2.3 The princeple of responsiveness

The principle of Vue response is to the use data jijacking combined with publisher-subscriber mode,  through Object.defineProperty to jijack the setter, getter of each property, publish a message to subscribers when the data changes, and trigger the corresponding listening callback. Mainly divided into the foloowing steps:

Objserve(hijacked data objects) Compile(vue Compilation) Watcher(subscribers) Dep(used to collect Watcher subscribers)

1. you need to recursively traverses the Observe data object, including child property object properties, add setter and getter such properties, to a value of the object assignment, will trigger the setter, then you can listen to the data changes.
2. Compile parses the template instruction, replaces the variables in the template with data, then intializes the rendering page view, and binds the update function to the node corresponding to each instruction, adds subscribers to monitor the data, once the data changes, receives a notification, update the view.
3. Watcher subscriber is a bridge between Observer and Compile communication, the main thing is to do is:
   1. in its own instantiation to the property subsciber(Dep)inside to add their own
   2. itself must have an update() method
   3. to be property changes dep.notice()notification, can call its own update() method, and trigger the Compile bound callback, it is successful.
4. MVVM as an entery point ofr data binding, intergrate Observer, Compile and Watcher, monitor your model data changes through Observer, parse Compilation template instruction through Compile, and finnally use Watcher to build a communication bridge between Observer and Compile to achieve data changes -> view updates; view interaction changes(input) -> two-way binding effect of data model changes.



3.1 How Object.defineProperty is used and what are the disadvantages

Oject.defineProperty (obj, prop, descriptor)

three parameters: 

Obj, the object to define

Prop, the property name or symbol to define or modify

Descriptor, the property descriptor (configuration object) to define or modify

Get the getter function of the property, or undefined if there is no getter. This function is called when the property is accessed. No parameters are passed during execution, but this object passsed(this is not neccessarily the object that defines the property due to inheritance).  The return value of the function is used as the value of the property. Defaults to to  [undefined] setter function of the set property,  or [undefined] if there is no setter. This function is called when the property value is modified. The method takes a paramater(that is, the new value assigned) and passes in the this object at assignment tiem. Defaults to [undefined].



Disadvantages:

When preforming operations on some properties, this method cannot be intercepted,  such as modifying array data by subscripting or adding properties to objects, which cannot tirgger the re-rendering of the component, because Object.defineProperty cannot intercept these operatioins. More precisely, for arrays, most operations cannot be intercepeted, but Vue internally solves this problem by rewriting funciton.

This method is no longer used in Vue 3.0, but data hijacking is achieved by using Proxy to proxy objects. The advantage of using Proxy is that it can prefectly listen for data changes in any way, the only disadvatage is compatibility issues, because Proxy is ES6 syntax.



3.2 Object.define Property (target, key, options), what parameters can option pass?

Value: set the initial value for target[key]

Get: triggered when target [key] is called

Set: tirggered when targert[key] is set

Writable: Specifies whether target[key] can be overridden, default false

Enumerable: Specifies whether the key will appear in the enumeration property of the target, the default is false

Configurable: Specifies whether the options can be changed, and the key attribute can be deleted. The default is false. 



4. MVVM, MVC, MVP

   1. MVC

   M: model data model, V: view view model, C: controller

   MVC organize the code structure by  searating Model, View and Controller. The View is responsible for the display logic of the page, and the Model is responsible for storing the business data of the page and the opration on the corresponding data. And View and Model apply the observer pattern, which will notify the page about the update of the View layer when the Model layer changes. Controller layer is the link between the View layer and the Model layer, which is mainly responsible for the response opreation between the user and the application. When the user interacts with the page, the event trigger in the Controller starts to work. By calling the Model layer, the Model is compledted.

   	2. MVVM

   M: the data model, and both data and business logic are defined in the model layer

   V: UI view, responsible for displaying data

   VM: mointornig the changes of data in the Model and controlling the update of the view, and handing user interaction.

​		Model and View are not directly related, but are related through ViewModel, and there is a two-way data bingding connection between Model and ViewModel. Therefore, when the data in the Model changes, the View layer will be refershed, and the data in the View that has changed due to user interaction will also be synchronized in the Model.

​	This mode enables automatic data synchronization between Model and View, so developers only need to focus on data matintenance opreations instead of opreating DOM themselves.



4.2 Advantages and disadvantages of MVVM ?

Advatages: 

​	Sepreate view and model, reduce code coupling, improve view or logic reusability: for example, view can be independent of model changes and modifications, a viewmodel can be bound to different views, when the view changes, the model cannnot be changed, and the view can also be changed when the model changes. You can put some view logic in a viewmodel and let many views reuse this view logic.

​	improve testability: ViewModel exists to help developers write better test code.

​	Automatic update dom: Using two-way binding, the view is automatically updated after data update, allowing developers to free themselves form tedious manual dom.



Disadvantages: 

Bugs are hard to debug: because of the two-way binding mode, when you see a boundary exception, it may be a bug in your view code, or it may be a bug in the model code. Data binding makes it easy to quickly pass bugs in one location to other locations, making it less easy to locate the original problem location. In addiction, the declaration of data binding is written in the templage of the view in instruction style, and there is no way to break the debug point.

In a large module, the model will also be very large. Although it is easy to use, it is easy to ensure the consistency of the data. At that time, holding it for a long time and not releasing the memory will cost more money.

For large graphical applications, with more view state, the cost of building and matinaing the viewmodel will be higher.

