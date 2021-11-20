# Chapter 4 Questions

## The first concern we have this chapter is forward and backwards compatibility. Talk about a time when you appreciated forwards compatibility, and a time when you appreciated backwards compatibility. What are some of the constraints put on application writers when forward or backwards compatibility is required? What are some of the ways that this can create errors for the user if not respected?

-- YOUR ANSWER HERE --

## What is the difference between JSON/XML and binary encodings? What are the pros and cons of JSON/XML vs Binary encoding? How do all the formats guarantee forward and backwards compatibility?

-- YOUR ANSWER HERE --

## How is Avro different from protobuf and Thrift? How does it guarantee forward and backwards compatibility?

-- YOUR ANSWER HERE --

## What are some challenges that occur when serialization and deserialization are done on data stored in a database? In web applications? In Mobile apps? In desktop apps? What are some of the challenges you've faced on making schemas forwards and backwards compatible in production?

-- YOUR ANSWER HERE --

## Weakening and Strengthening Conditions

Let's say I have a function that takes an `x` and returns the serialized
form of `x`, which we call `y`. I know that this call can fail, so this
function returns a Maybe.

In Haskell, a Maybe type is defined as: `Maybe a = Just a | Nothing`.
A Maybe type may either have that `a` or not.

```hs
-- This means the serialize function takes an `x` and possibly returns a
`y`, otherwise it returns Nothing.
Serialize :: x -> Maybe y
```

Let's say I have discovered that this call will never fail. I strengthen
my precondition to make sure that it returns a y, always.

```hs
-- This means the serialize function takes an `x` and always returns a
`y`.
`y`, otherwise it returns Nothing.
Serialize :: x -> y
```

What happens to previous callers who assumed they had to check that y
could fail?

What happens if I weaken a condition?

```hs
-- This means the serialize function takes an `x` and always returns a
`y`.
Serialize :: x -> y
```

I thought the caller would need to provide an `x`, but I later on
provided a default `y` which can be returned if `x` is not provided.

```hs
-- This means the serialize function can take an `x` and always returns a
`y`.
Serialize :: Maybe x -> y
```

What happens to callers that had previously provided a Maybe type?

-- YOUR ANSWER HERE --

## In protobuf3, the `required` keyword for schemas was removed. More context here: [Required is removed from protobuf 3](https://stackoverflow.com/a/31814967). In your own words, why do you think it was removed? What are the pros and cons of having a "required" field for backwards and forwards compatibility?

-- YOUR ANSWER HERE --
