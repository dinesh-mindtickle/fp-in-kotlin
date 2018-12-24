# Functional Programming in Kotlin

[Arrow](https://arrow-kt.io/) provides great tools to implement FP in Kotlin. While writing this document, I have extremely limited of FP and Arrow. Treat this document as a pure experiment.

**Exception handling: The Functional way**

For starters, we shall start with Exceptional handling

To understand the beauty of exception handling in Functional programming, please visit this link: https://arrow-kt.io/docs/arrow/core/try/

I have created extension ```toTry()``` function for ```Single```, ```Observable``` and ```Flowable```. Any RxJava stream ```Flowable<T>``` can be converted to ```Flowable<Try>``` by calling the above mentioned extension function ```toTry()```

The definition of ```toTry()``` is as follows:

```
fun <T> Observable<T>.toTry(): Observable<Try<T>>{
    return this.map { response -> Try{ response} }.onErrorReturn { error -> Try.raise(error) }
}

fun <T> Flowable<T>.toTry(): Flowable<Try<T>>{
    return this.map { response -> Try{ response} }.onErrorReturn { error -> Try.raise(error) }
}

fun <T> Single<T>.toTry(): Single<Try<T>>{
    return this.map { response -> Try{ response} }.onErrorReturn { error -> Try.raise(error) }
}
```

You can use fold to get the data or handling exception:

      response.fold(
                    {error ->
                        //Handle the error
                        Timber.e(error, "Error while fetching data from server")
                    }, {
                    data ->
                    //Handle the data
                })

