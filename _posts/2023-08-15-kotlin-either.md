---
layout: post
title: Writing a Either Class in Kotlin
tags: software
---

## Definition

```kotlin
sealed class Either<out T, out E>

data class Success<out T>(val value: T) : Either<T, Nothing>()
data class Error<out E>(val error: E) : Either<Nothing, E>()
```

## Custom Errors

```kotlin
sealed class ThingError

object GetThingError : ThingError()

data class ThingNotFoundError(val thing: Thing) : ThingError()
```

## Check for Success

```kotlin
val thingOrError: Either<Thing, ThingError> = getThingOrError()

if (thingOrError is Success) {
    val thing: Thing = thingOrError.value
    // handle thing
}
```

## Check for Error

```kotlin
val thingOrError: Either<Thing, ThingError> = getThingOrError()

if (thingOrError is Error) {
    val error: ThingError = thingOrError.error
    // handle error
}
```

## Get Or Null

```kotlin
val thingOrError = getThingOrError()

val thingOrNull: Thing? = when(thingOrError) {
    is Success -> thingOrError.value
    is Error -> null
}
```

## Error Or Null

```kotlin
val thingOrError = getThingOrError()

val errorOrNull: ThingError? = when(thingOrError) {
    is Success -> null
    is Error -> thingOrError.error
}
```

## Interop with Kotlin Result, runCatching

```kotlin
val result: Result<Thing> = runCatching {
    getThingOrThrow()
}

val thingOrError = result.getOrNull()?.let { Success(it) } ?: Error(GetThingError)
```

## Pass Errors

```kotlin
val thingOrError = getThingOrError()

val otherThingOrError = when(thingOrError) {
    is Success -> {
        val thing = thingOrError.value
        val otherThing = thing.toOtherThing()
        Success(otherThing)
    }
    is Error -> thingOrError
}
```

## Handle List of Eithers

```kotlin
val things = getThings()

val otherThingOrErrors = things.map { thing -> thing.getOtherThingOrError() }

if (otherThingOrErrors.all { it is Success }) {
    val otherThings = otherThingOrErrors.map { (it as Success).value }
}

if (otherThingOrErrors.any { it is Error }) {
    val error = otherThingOrErrors.first { it is Error } as Error
}
```