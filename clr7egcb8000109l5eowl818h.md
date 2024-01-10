---
title: "[Docs Review]LiveData overview"
datePublished: Wed Jan 10 2024 06:28:29 GMT+0000 (Coordinated Universal Time)
cuid: clr7egcb8000109l5eowl818h
slug: docs-reviewlivedata-overview

---

[`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) is an observable data holder class. Unlike a regular observable, **LiveData is lifecycle-aware, meaning it respects the lifecycle of other app components, such as activities, fragments, or services. This awareness ensures LiveData only updates app component observers that are in an active lifecycle state.**

> **Note:** To import LiveData components into your Android project, see [Adding Components to your Project](https://developer.android.com/topic/libraries/architecture/adding-components#lifecycle).

LiveData considers an observer, which is represented by the [`Observer`](https://developer.android.com/reference/androidx/lifecycle/Observer) class, to be in an active state if its lifecycle is in the [`STARTED`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle.State#STARTED) or [`RESUMED`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle.State#RESUMED) state. LiveData only notifies active observers about updates. Inactive observers registered to watch [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) objects aren't notified about changes.

You can register an observer paired with an object that implements the [`LifecycleOwner`](https://developer.android.com/reference/androidx/lifecycle/LifecycleOwner) interface. This relationship allows the observer to be removed when the state of the corresponding [`Lifecycle`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle) object changes to [`DESTROYED`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle.State#DESTROYED). This is especially useful for activities and fragments because they can safely observe [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) objects and not worry about leaks—activities and fragments are instantly unsubscribed when their lifecycles are destroyed.

## The advantage of using LiveData

Using LiveData provides the following advantages:

### **Ensures your UI matches your data state**

LiveData follows the observer pattern. LiveData notifies [`Observer`](https://developer.android.com/reference/androidx/lifecycle/Observer) objects when underlying data changes. You can consolidate your code to update the UI in these `Observer` objects. That way, you don't need to update the UI every time the app data changes because the observer does it for you.

### No memory leaks

Observers are bound to [`Lifecycle`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle) objects and clean up after themselves when their associated lifecycle is destroyed.

### No crashes due to stopped activities

If the observer's lifecycle is inactive, such as in the case of an activity in the back stack, **then it doesn’t receive any LiveData events.**

### No more manual lifecycle handing

UI components just observe relevant data and don’t stop or resume observation. LiveData automatically manages all of this since it’s aware of the relevant lifecycle status changes while observing.

### Always up to date data

If a lifecycle becomes inactive, it receives the latest data upon becoming active again. For example, an activity that was in the background receives the latest data right after it returns to the foreground.

### Proper configuration changes

If an activity or fragment is recreated due to a configuration change, like device rotation, it immediately receives the latest available data.

### Sharing resources

You can extend a `LiveData` object using the singleton pattern to wrap system services so that they can be shared in your app. The LiveData object connects to the system service once, and then any observer that needs the resource can just watch the LiveData object. For more information, see Extend LiveData.

# Work with LiveData objects

Follow these steps to work with [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) objects:

1. Create an instance of `LiveData` to hold a certain type of data. This is usually done within your [`ViewModel`](https://developer.android.com/reference/androidx/lifecycle/ViewModel) class.
    
2. Create an [`Observer`](https://developer.android.com/reference/androidx/lifecycle/Observer) object that defines the [`onChanged()`](https://developer.android.com/reference/androidx/lifecycle/Observer#onChanged(T)) method, which controls what happens when the `LiveData` object's held data changes. You usually create an `Observer` object in a UI controller, such as an activity or fragment.
    
3. Attach the `Observer` object to the `LiveData` object using the [`observe()`](https://developer.android.com/reference/androidx/lifecycle/LiveData#observe(androidx.lifecycle.LifecycleOwner,%0Aandroidx.lifecycle.Observer%3CT%3E)) method. The `observe()` method takes a [`LifecycleOwner`](https://developer.android.com/reference/androidx/lifecycle/LifecycleOwner) object. This subscribes the `Observer` object to the `LiveData` object so that it is notified of changes. You usually attach the `Observer` object in a UI controller, such as an activity or fragment.
    

> **Note:** You can register an observer without an associated [`LifecycleOwner`](https://developer.android.com/reference/androidx/lifecycle/LifecycleOwner) object using the [`observeForever(Observer)`](https://developer.android.com/reference/androidx/lifecycle/LiveData#observeForever(androidx.lifecycle.Observer%3CT%3E)) method. In this case, the observer is considered to be always active and is therefore always notified about modifications. You can remove these observers calling the [`removeObserver(Observer)`](https://developer.android.com/reference/androidx/lifecycle/LiveData#removeObserver(androidx.lifecycle.Observer%3CT%3E)) method.

When you update the value stored in the `LiveData` object, it triggers all registered observers as long as the attached `LifecycleOwner` is in the active state.

LiveData allows UI controller observers to subscribe to updates. When the data held by the `LiveData` object changes, the UI automatically updates in response.

## Create LiveData objects

LiveData is a wrapper that can be used with any data, including objects that implement [`Collections`](https://developer.android.com/reference/java/util/Collections), such as [`List`](https://developer.android.com/reference/java/util/List). A [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) object is usually stored within a [`ViewModel`](https://developer.android.com/reference/androidx/lifecycle/ViewModel) object and is accessed via a getter method, as demonstrated in the following example:

```kotlin
class NameViewModel : ViewModel() {

    // Create a LiveData with a String
    val currentName: MutableLiveData<String> by lazy {
        MutableLiveData<String>()
    }

    // Rest of the ViewModel...
}
```

Initially, the data in a `LiveData` object is not set.

> **Note:** Make sure to store `LiveData` objects that update the UI in `ViewModel` objects, as opposed to an activity or fragment, for the following reasons:
> 
> * To avoid bloated activities and fragments. Now these UI controllers are responsible for displaying data but not holding data state.
>     
> * To decouple `LiveData` instances from specific activity or fragment instances and allow `LiveData` objects to survive configuration changes.
>     

## Observe LiveData objects

In most cases, an app component’s [`onCreate()`](https://developer.android.com/reference/android/app/Activity#onCreate(android.os.Bundle)) method is the right place to begin observing a [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) object for the following reasons:

* To ensure the system doesn’t make redundant calls from an activity or fragment’s [`onResume()`](https://developer.android.com/reference/android/app/Activity#onResume()) method.
    
* To ensure that the activity or fragment has data that it can display as soon as it becomes active. As soon as an app component is in the [`STARTED`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle.State#STARTED) state, it receives the most recent value from the `LiveData` objects it’s observing. This only occurs if the `LiveData` object to be observed has been set.
    

Generally, LiveData delivers updates only when data changes, and only to active observers. **An exception to this behavior is that observers also receive an update when they change from an inactive to an active state.** Furthermore, **if the observer changes from inactive to active a second time, it only receives an update if the value has changed since the last time it became active.**

The following sample code illustrates how to start observing a `LiveData` object:

```kotlin
class NameActivity : AppCompatActivity() {

    // Use the 'by viewModels()' Kotlin property delegate
    // from the activity-ktx artifact
    private val model: NameViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Other code to setup the activity...

        // Create the observer which updates the UI.
        val nameObserver = Observer<String> { newName ->
            // Update the UI, in this case, a TextView.
            nameTextView.text = newName
        }

        // Observe the LiveData, passing in this activity as the LifecycleOwner and the observer.
        model.currentName.observe(this, nameObserver)
    }
}
```

> After [`observe()`](https://developer.android.com/reference/androidx/lifecycle/LiveData#observe(androidx.lifecycle.LifecycleOwner,%0Aandroidx.lifecycle.Observer%3CT%3E)) is called with `nameObserver` passed as parameter, [`onChanged()`](https://developer.android.com/reference/androidx/lifecycle/Observer#onChanged(T)) is immediately invoked providing the most recent value stored in `mCurrentName`. If the `LiveData` object hasn't set a value in `mCurrentName`, `onChanged()` is not called.

## Update LiveData objects

LiveData has no publicly available methods to update the stored data. The [`MutableLiveData`](https://developer.android.com/reference/androidx/lifecycle/MutableLiveData) class exposes the [`setValue(T)`](https://developer.android.com/reference/androidx/lifecycle/MutableLiveData#setValue(T)) and [`postValue(T)`](https://developer.android.com/reference/androidx/lifecycle/MutableLiveData#postValue(T)) methods publicly and you must use these if you need to edit the value stored in a [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) object. Usually `MutableLiveData` is used in the [`ViewModel`](https://developer.android.com/reference/androidx/lifecycle/ViewModel) and then the `ViewModel` only exposes immutable `LiveData` objects to the observers.

After you have set up the observer relationship, you can then update the value of the `LiveData` object, as illustrated by the following example, which triggers all observers when the user taps a button:

```kotlin
button.setOnClickListener {
    val anotherName = "John Doe"
    model.currentName.setValue(anotherName)
}
```

> Calling `setValue(T)` in the example results in the observers calling their [`onChanged()`](https://developer.android.com/reference/androidx/lifecycle/Observer#onChanged(T)) methods with the value `John Doe.`

The example shows a button press, but `setValue()` or `postValue()` could be called to update `mName` for a variety of reasons, including in response to a network request or a database load completing; in all cases, the call to `setValue()` or `postValue()` triggers observers and updates the UI.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><strong>Note</strong>: You must call the setValue(T) method to update the LiveData object from the main thread. If the code is executed in a worker thread, you can use the postValue(T) method instead to update the LiveData object.</div>
</div>

## Use LiveData objects

Room generates all the necessary code to update the `LiveData` object when a database is updated. The generated code runs the query asynchronously on a background thread when needed. This pattern is useful for keeping the data displayed in a UI in sync with the data stored in a database. You can read more about Room and DAOs in the [Room persistent library guide](https://developer.android.com/topic/libraries/architecture/room).

## Use coroutines with LiveData

`LiveData` includes support for Kotlin coroutines. For more information, see [Use Kotlin coroutines with Android Architecture Components](https://developer.android.com/topic/libraries/architecture/coroutines).

# LiveData in an app's architecture

`LiveData` is lifecycle-aware, following the lifecycle of entities such as activities and fragments. Use `LiveData` to communicate between these lifecycle owners and other objects with a different lifespan, such as `ViewModel` objects. The main responsibility of the `ViewModel` is to load and manage UI-related data, which makes it a great candidate for holding `LiveData` objects. Create `LiveData` objects in the `ViewModel` and use them to expose state to the UI layer.

Activities and fragments should not hold `LiveData` instances because their role is to display data, not hold state. Also, keeping activities and fragments free from holding data makes it easier to write unit tests.

It may be tempting to work `LiveData` objects in your data layer class, but `LiveData`is not designed to handle asynchronous streams of data. Even though you can use `LiveData` transformations and [`MediatorLiveData`](https://developer.android.com/reference/androidx/lifecycle/MediatorLiveData) to achieve this, this approach has drawbacks: the capability to combine streams of data is very limited and all `LiveData` objects (including ones created through transformations) are observed on the main thread. The code below is an example of how holding a `LiveData` in the `Repository` can block the main thread:

```kotlin
class UserRepository {

    // DON'T DO THIS! LiveData objects should not live in the repository.
    fun getUsers(): LiveData<List<User>> {
        ...
    }

    fun getNewPremiumUsers(): LiveData<List<User>> {
        return getUsers().map { users ->
            // This is an expensive call being made on the main thread and may
            // cause noticeable jank in the UI!
            users
                .filter { user ->
                  user.isPremium
                }
          .filter { user ->
              val lastSyncedTime = dao.getLastSyncedTime()
              user.timeCreated > lastSyncedTime
                }
    }
}
```

> If you need to use streams of data in other layers of your app, consider using [Kotlin Flows](https://developer.android.com/kotlin/flow) and then converting them to `LiveData` in the `ViewModel` using [`asLiveData()`](https://developer.android.com/reference/kotlin/androidx/lifecycle/package-summary#aslivedata).

Learn more about using Kotlin `Flow` with `LiveData` in [this codelab](https://developer.android.com/codelabs/advanced-kotlin-coroutines). For codebases built with Java, consider using [Executors](https://developer.android.com/guide/background/threading) in conjunction with callbacks or `RxJava`.

# Extend LiveData

LiveData considers an observer to be in an active state if the observer's lifecycle is in either the [`STARTED`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle.State#STARTED) or [`RESUMED`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle.State#RESUMED) states. The following sample code illustrates how to extend the [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) class:

```kotlin
class StockLiveData(symbol: String) : LiveData<BigDecimal>() {
    private val stockManager = StockManager(symbol)

    private val listener = { price: BigDecimal ->
        value = price
    }

    override fun onActive() {
        stockManager.requestPriceUpdates(listener)
    }

    override fun onInactive() {
        stockManager.removeUpdates(listener)
    }
}
```

The implementation of the price listener in this example includes the following important methods:

* The [`onActive()`](https://developer.android.com/reference/androidx/lifecycle/LiveData#onActive()) method is called when the `LiveData` object has an active observer. This means you need to start observing the stock price updates from this method.
    
* The [`onInactive()`](https://developer.android.com/reference/androidx/lifecycle/LiveData#onInactive()) method is called when the `LiveData` object doesn't have any active observers. Since no observers are listening, there is no reason to stay connected to the `StockManager` service.
    
* The [`setValue(T)`](https://developer.android.com/reference/androidx/lifecycle/MutableLiveData#setValue(T)) method updates the value of the `LiveData` instance and notifies any active observers about the change.
    

You can use the `StockLiveData` class as follows:

```kotlin
public class MyFragment : Fragment() {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        val myPriceListener: LiveData<BigDecimal> = ...
        myPriceListener.observe(viewLifecycleOwner, Observer<BigDecimal> { price: BigDecimal? ->
            // Update the UI.
        })
    }
}
```

The [`observe()`](https://developer.android.com/reference/androidx/lifecycle/LiveData#observe(androidx.lifecycle.LifecycleOwner,%20androidx.lifecycle.Observer%3C?%20super%20T%3E)) method passes the [`LifecycleOwner`](https://developer.android.com/reference/androidx/lifecycle/LifecycleOwner) associated with the fragment's view as the first argument. Doing so denotes that this observer is bound to the [`Lifecycle`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle) object associated with the owner, meaning:

* If the `Lifecycle` object is not in an active state, then the observer isn't called even if the value changes.
    
* After the `Lifecycle` object is destroyed, the observer is automatically removed.
    

The fact that `LiveData` objects are lifecycle-aware means that you can share them between multiple activities, fragments, and services. To keep the example simple, you can implement the `LiveData` class as a singleton as follows:

```kotlin
class StockLiveData(symbol: String) : LiveData<BigDecimal>() {
    private val stockManager: StockManager = StockManager(symbol)

    private val listener = { price: BigDecimal ->
        value = price
    }

    override fun onActive() {
        stockManager.requestPriceUpdates(listener)
    }

    override fun onInactive() {
        stockManager.removeUpdates(listener)
    }

    companion object {
        private lateinit var sInstance: StockLiveData

        @MainThread
        fun get(symbol: String): StockLiveData {
            sInstance = if (::sInstance.isInitialized) sInstance else StockLiveData(symbol)
            return sInstance
        }
    }
}
```

And you can use it in the fragment as follows:

```kotlin
class MyFragment : Fragment() {

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        StockLiveData.get(symbol).observe(viewLifecycleOwner, Observer<BigDecimal> { price: BigDecimal? ->
            // Update the UI.
        })

    }
```

Multiple fragments and activities can observe the `MyPriceListener` instance. LiveData only connects to the system service if one or more of them is visible and active.

# Transform LiveData

You may want to make changes to the value stored in a LiveData object before dispatching it to the observers, or you may need to return a different LiveData instance based on the value of another one. The Lifecycle package provides the Transformations class which includes helper methods that support these scenarios.

### Transformations.map()

> The `map()` function in LiveData works differently than the familiar map function in collections. It's more akin to transforming data from the original LiveData to a new type of LiveData.

Applies a function on the value stored in the LiveData object, and propagates the result downstream.

```kotlin
val userLiveData: LiveData<User> = UserLiveData()
val userName: LiveData<String> = userLiveData.map {
    user -> "${user.name} ${user.lastName}"
}
```

### Transformations.switchMap()

Similar to `map()`, applies a function to the value stored in the `LiveData` object and unwraps and dispatches the result downstream. The function passed to `switchMap()` must return a `LiveData` object, as illustrated by the following example:

```kotlin
private fun getUser(id: String): LiveData<User> {
  ...
}
val userId: LiveData<String> = ...
val user = userId.switchMap { id -> getUser(id) }
```

You can use transformation methods to carry information across the observer's lifecycle. **The transformations aren't calculated unless an observer is watching the returned** `LiveData` **object(user: LiveData&lt;User&gt;).** Because the transformations are calculated lazily, lifecycle-related behavior is implicitly passed down without requiring additional explicit calls or dependencies.

**If you think you need a** `Lifecycle` **object inside a** [`ViewModel`](https://developer.android.com/reference/androidx/lifecycle/ViewModel) **object, a transformation is probably a better solution.** For example, assume that you have a UI component that accepts an address and returns the postal code for that address. You can implement the naive `ViewModel` for this component as illustrated by the following sample code:

```kotlin
class MyViewModel(private val repository: PostalCodeRepository) : ViewModel() {

    private fun getPostalCode(address: String): LiveData<String> {
        // DON'T DO THIS
        return repository.getPostCode(address)
    }
}
```

> The UI component then needs to unregister from the previous `LiveData` object and register to the new instance each time it calls `getPostalCode()`. In addition, if the UI component is recreated, it triggers another call to the `repository.getPostCode()` method instead of using the previous call’s result.

Instead, you can implement the postal code lookup as a transformation of the address input, as shown in the following example:

```kotlin
class MyViewModel(private val repository: PostalCodeRepository) : ViewModel() {
    private val addressInput = MutableLiveData<String>()
    val postalCode: LiveData<String> = addressInput.switchMap {
            address -> repository.getPostCode(address) }


    private fun setInput(address: String) {
        addressInput.value = address
    }
}
```

> In this case, the `postalCode` field is defined as a transformation of the `addressInput`. As long as your app has an active observer associated with the `postalCode` field, the field's value is recalculated and retrieved whenever `addressInput` changes.
> 
> In this case, the `postalCode` field is defined as a transformation of the `addressInput`. As long as your app has an active observer associated with the `postalCode` field, the field's value is recalculated and retrieved whenever `addressInput` changes.

## Create new transformations

There are a dozen different specific transformation that may be useful in your app, but they aren’t provided by default. **To implement your own transformation you can you use the** [`MediatorLiveData`](https://developer.android.com/reference/androidx/lifecycle/MediatorLiveData) **class, which listens to other** [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) **objects and processes events emitted by them**. `MediatorLiveData` correctly propagates its state to the source `LiveData` object. To learn more about this pattern, see the reference documentation of the [`Transformations`](https://developer.android.com/reference/androidx/lifecycle/Transformations) class.

# Merge multiple LiveData sources

[`MediatorLiveData`](https://developer.android.com/reference/androidx/lifecycle/MediatorLiveData) **is a subclass of** [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) **that allows you to merge multiple LiveData sources. Observers of** `MediatorLiveData` **objects are then triggered whenever any of the original LiveData source objects change.**

For example, if you have a `LiveData` object in your UI that can be updated from a local database or a network, then you can add the following sources to the `MediatorLiveData` object:

* A `LiveData` object associated with the data stored in the database.
    
* A `LiveData` object associated with the data accessed from the network.
    

Your activity only needs to observe the `MediatorLiveData` object to receive updates from both sources. For a detailed example, see the [Addendum: exposing network status](https://developer.android.com/topic/libraries/architecture/guide#addendum) section of the [Guide to App Architecture](https://developer.android.com/topic/libraries/architecture/guide).

# Additional resources

To learn more about the [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData) class, consult the following resources.

### Samples

* [Sunflower](https://github.com/android/sunflower), a demo app demonstrating best practices with Architecture Components
    
* [Android Architecture Components Basic Sample](https://github.com/android/architecture-components-samples/tree/main/BasicSample)
    

### Codelabs

* Android Room with a View [(Java)](https://codelabs.developers.google.com/codelabs/android-room-with-a-view) [(Kotlin)](https://codelabs.developers.google.com/codelabs/android-room-with-a-view-kotlin)
    
* [Learn advanced coroutines with Kotlin Flow and LiveData](https://developer.android.com/codelabs/advanced-kotlin-coroutines)
    

### Blogs

* [ViewModels and LiveData: Patterns + AntiPatterns](https://medium.com/androiddevelopers/viewmodels-and-livedata-patterns-antipatterns-21efaef74a54)
    
* [LiveData beyond the ViewModel — Reactive patterns using Transformations and MediatorLiveData](https://medium.com/androiddevelopers/livedata-beyond-the-viewmodel-reactive-patterns-using-transformations-and-mediatorlivedata-fda520ba00b7)
    
* [LiveData with SnackBar, Navigation and other events (the SingleLiveEvent case)](https://medium.com/androiddevelopers/livedata-with-snackbar-navigation-and-other-events-the-singleliveevent-case-ac2622673150)
    

### Videos

* [Jetpack LiveData](https://www.youtube.com/watch?v=OMcDk2_4LSk)
    
* [Fun with LiveData (Android Dev Summit '18)](https://www.youtube.com/watch?v=2rO4r-JOQtA)
    

# References

[https://developer.android.com/topic/libraries/architecture/livedata](https://developer.android.com/topic/libraries/architecture/livedata)

---

*'Portions of this page are reproduced from work created and* [*shared by the Android Open Source Project*](http://code.google.com/policies.html) *and used according to terms described in the* [*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/)*.*