## Exam question:
*Describe data binding and the MVVM model. Explain how MVVM relates to events, the Command pattern and the Observer pattern. Compare this architecture to the ASP.NET MVC architecture.*

### Presentation
Alright, so let's dive into MVVM.
<img src="assets/mvvm_2.png" />
It's all about separation of concerns. The View part is XAML based with a bit of *code-behind* and is data-bound to the ViewModel. The ViewModel then holds the actual state and operations. It receives change notifications from the model, and so does the View through declarative data binding. It then updates the Model accordingly upon user interactions and what not.

### Advantages:
- It is very easy to simply test the logic behind the View (the ViewModel).

- It is very easy to just swap out the actual view (the XAML-thingy) which makes it perfect for switching technologies or delivering different view components to different devices (which is perfect for Microsoft's Universal Platform strategy).

- It provides separation of concerns in a similar fashion to MVC.

### Keep the Code-Behind small
In order to achieve the separation properties of MVVM, we have to keep to footprint of the code-behind file as minimal as possible. This also means that we shouldn't place any event handlers there. Instead, the ViewModel should decide which actions should be handled by implementing the `ICommand` interface. The View should then bind to these actions to be able to relay the actual handling of the actions to the ViewModel instead.

(### Difference from MVC
In MVC, The controller knows about the View and the Model.
It makes sure to update the view when the model changes. And that might happen as a result of a user interaction such as a button that was clicked or something like that.

In terms of ASP.NET MVC, the controller receives requests and responds to requests. In MVVM. Staying with that analogy, here the View receives the request, but it never actually handles it.)

In MVVM, the View knows about the ViewModel and the ViewModel knows about the Model, the model doesn't know anything about the ViewModel and the ViewModel doesn't know anything about the view. This on its own means that MVVM is less tightly coupled than MVC.

### Relation to the Observer Pattern
It is very much related to the Observer Pattern, but the abstraction is a bit different. Here, the View observes the ViewModel through the abstraction of data binding. It can, however, also directly manipulate the `ViewModel` through TwoWay binding, but let's get back to that.

When the ViewModel change, it will notify the view which will update accordingly.

The ViewModel observers the model as well. This means that if the model changes something that the ViewModel is observing, and the View happens to bind that property, it too will be updated.

### Relation to the Command Pattern
By having the View completely isolated from the Model, we make sure that all requests to update the state happens through an intermediary. For instance, instead of having a button click directly alter the state of the model, we instead handle the request in the ViewModel. This also enables us to, for instance, introduce undo/redo or logging at a later point, because now we have full control over the request before fulfilling it.
