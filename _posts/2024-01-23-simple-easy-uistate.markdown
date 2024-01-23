---
layout: post
title: UiState - Centralize you screen states in a simple way
date: 2023-12-27 12:00:00 +0300
description: Learn how to implement uiState in a straightforward manner
img: kotlin.jpg 
tags: [Kotlin, UiState, Optimize]
---

UI state encompasses the current configuration and behavior of an application's visible components. This includes the visibility of various elements, user inputs, loading indicators, and other factors that determine how the interface responds to user actions. Understanding and effectively managing UI state is crucial for creating a consistent and intuitive user experience. This approach ensures that the application behaves predictably in different scenarios, and in Android, it allows us to reduce the use of LiveData and observables, centralizing UI changes in a single point.

### Why Use uiState in Kotlin?

Kotlin's suitability for UI state management arises from its support for immutability through features like data classes. Immutability ensures that UI state changes are handled consistently and without unexpected side effects. Additionally, Kotlin embraces reactive programming paradigms, such as Kotlin Flow, providing a powerful toolset for gracefully handling asynchronous UI state changes. This combination of language features and reactive capabilities makes Kotlin an excellent choice for effective UI state management.

### First step: Create the Screen States

In practice, uiState is used as follows: we have a LiveData that receives the states, and the screen observes only this variable, reacting according to each update.

Firstly, we will create the screen states. In the example, I will create a state passing parameters (data class) and a state without passing parameters (data object).



```kotlin
sealed class ExampleUiState {
    data object Success : ExampleUiState()
    data class Error(val error: IndicationErrorEnum) : ExampleUiState()
}
```

### Second step: Create our LiveData on ViewModel

Now we need to create the central LiveData that will feed our view with the desired state updates.

The setState function will be used to send updates to the screen.

```kotlin
class ExampleViewModel : ViewModel() {

    private val uiState: MutableLiveData<ExampleUiState> = MutableLiveData()
    fun state(): LiveData<ExampleUiState> = uiState

    fun setState(state: ExampleUiState) {
        viewModelScope.launch(Dispatchers.Main) {
            uiState.value = state
        }
    }
}
```


### Final step: Monitoring the Updates

In our view, we will now observe the changes and update our screen accordingly. Remember that in the state, we can pass variables, avoiding unnecessary ViewModel queries with each state change.

```kotlin
    private fun setupObserver() {
        viewModel.state().observe(this) { state ->
            when (state) {
                is ExampleUiState.Success -> this.showSuccessState()
                is ExampleUiState.Error -> this.showErrorState(state.error)
            }
        }
    } 
```

### Conclusion

In conclusion, implementing uiState in Kotlin provides a clean and efficient way to manage the state of your user interface. By centralizing screen states and using Kotlin's features such as immutability and reactive programming, developers can create more predictable and maintainable UIs, ultimately enhancing the overall user experience.