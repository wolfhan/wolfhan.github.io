---
title: Use of ViewModel with LiveData in Android
date: 2019-01-10 00:21:58
tags:
categories: android
---
In [Android Architecture Components](https://developer.android.com/topic/libraries/architecture/),  [`ViewModel`](https://developer.android.com/reference/android/arch/lifecycle/ViewModel.html) and [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html) are used to change values and notify   observables with respect to android lifecycle. 

> The [`ViewModel`](https://developer.android.com/reference/android/arch/lifecycle/ViewModel.html) class is designed to store and manage UI-related data in a lifecycle conscious way. The [`ViewModel`](https://developer.android.com/reference/android/arch/lifecycle/ViewModel.html) class allows data to survive configuration changes such as screen rotations.

> [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html) is an observable data holder class. Unlike a regular observable, LiveData is lifecycle-aware, meaning it respects the lifecycle of other app components, such as activities, fragments, or services. This awareness ensures LiveData only updates app component observers that are in an active lifecycle state.

<!-- more -->

### How to use ViewModel with LiveData

Follow these steps to work with [`ViewModel`](https://developer.android.com/reference/android/arch/lifecycle/ViewModel.html) and [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html) objects:

1. Create an instance of [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html) to hold a certain type of data. This is usually done within your [`ViewModel`](https://developer.android.com/reference/android/arch/lifecycle/ViewModel.html) class.

   ```kotlin
   class NameViewModel : ViewModel() {
   
       // Create a LiveData with a String
       val currentName: MutableLiveData<String> by lazy {
           MutableLiveData<String>()
       }
   
       // Rest of the ViewModel...
   }
   ```

2. Create an [`Observer`](https://developer.android.com/reference/android/arch/lifecycle/Observer.html) object that defines the [`onChanged()`](https://developer.android.com/reference/android/arch/lifecycle/Observer.html#onChanged(T)) method, which controls what happens when the [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html) object's held data changes. You usually create an [`Observer`](https://developer.android.com/reference/android/arch/lifecycle/Observer.html) object in a UI controller, such as an activity or fragment.

   ```kotlin
   class NameActivity : AppCompatActivity() {
   
       private lateinit var mModel: NameViewModel
   
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
   
           // Other code to setup the activity...
   
           // Get the ViewModel.
           mModel = ViewModelProviders.of(this).get(NameViewModel::class.java)
   
   
           // Create the observer which updates the UI.
           val nameObserver = Observer<String> { newName ->
               // Update the UI, in this case, a TextView.
               mNameTextView.text = newName
           }
   
           // Observe the LiveData, passing in this activity as the LifecycleOwner and the observer.
           mModel.currentName.observe(this, nameObserver)
       }
   }
   ```

3. Attach the [`Observer`](https://developer.android.com/reference/android/arch/lifecycle/Observer.html) object to the [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html) object using the [`observe()`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html#observe(android.arch.lifecycle.LifecycleOwner,%0Aandroid.arch.lifecycle.Observer%3CT%3E)) method. The [`observe()`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html#observe(android.arch.lifecycle.LifecycleOwner,%0Aandroid.arch.lifecycle.Observer%3CT%3E)) method takes a [`LifecycleOwner`](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html) object. This subscribes the [`Observer`](https://developer.android.com/reference/android/arch/lifecycle/Observer.html) object to the [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html) object so that it is notified of changes. You usually attach the [`Observer`](https://developer.android.com/reference/android/arch/lifecycle/Observer.html) object in a UI controller, such as an activity or fragment.

   ```kotlin
   mButton.setOnClickListener {
       val anotherName = "John Doe"
       mModel.currentName.setValue(anotherName)
   }
   ```

This is a simple example that shows you how to use ViewModel and LiveData in Android. You can find more details in [this](https://developer.android.com/topic/libraries/architecture/livedata).