# Chapter 4 Questions

## The first concern we have this chapter is forward and backwards compatibility. Talk about a time when you appreciated forwards compatibility, and a time when you appreciated backwards compatibility. What are some of the constraints put on application writers when forward or backwards compatibility is required? What are some of the ways that this can create errors for the user if not respected?

I can’t think of a specific example, but I’m sure I’ve benefited from forwards compatibility on the web. For example I can send a request with a new JSON field, and an older service can just ignore it so that it is forward compatible. 

I appreciated backwards compatibility when I used to play old video games on the newer versions of the consoles. 

Some constraints: application writers often need to write explicit logic to handle old data for backwards compatibility. They may also need to maintain multiple versions of some API to accommodate different clients. 

Imagine a scenario where a user has some client application that communicates with a service that was recently updated. If there are compatibility issues, the user may encounter errors until they have upgraded to the latest version.


## What is the difference between JSON/XML and binary encodings? What are the pros and cons of JSON/XML vs Binary encoding? How do all the formats guarantee forward and backwards compatibility?

The main difference between JSON/XML and binary encoding is that JSON and XML are human readable while binary encoded data is not. However, binary encodings are more compact and allow for faster parsing.

One way that binary encodings allow for forwards / backwards compatibility is by allowing multiple schemas. You can map a version number to a schema so that you can look up schemas from past / future versions, or with Avro you can dynamically generate schemas and make conversions between them.


## How is Avro different from protobuf and Thrift? How does it guarantee forward and backwards compatibility?

The main difference is how their schemas work. While protobuf and Thrift use tag numbers in their schemas to enforce compatibility, Avro doesn’t use tags. This allows for more flexible encoding. Avro achieves forward / backwards compatibility by using default values to convert between different schemas. 

## What are some challenges that occur when serialization and deserialization are done on data stored in a database? In web applications? In Mobile apps? In desktop apps? What are some of the challenges you've faced on making schemas forwards and backwards compatible in production?

Databases: 
- Schemas can change over time. If a new field is added to the schema and some old code tries to update a record in the database, you need to ensure that the new field isn’t lost in the translation process. 

Web / mobile / desktop apps:
- All of these apps can make network requests to a server. One potential issue is that the client and service may be implemented in different programming languages, which can require some ugly translation of data types. 
- Service owners cannot force clients to upgrade, so they may have to maintain multiple API versions if a compatibility breaking change is required. 


## Weakening and Strengthening Conditions

Let's say I have a function that takes an `x` and returns the serialized
form of `x`, which we call `y`. I know that this call can fail, so this
function returns a Maybe.

In Haskell, a Maybe type is defined as: `Maybe a = Just a | Nothing`.
A Maybe type may either have that `a` or not.

```hs
-- This means the serialize function takes an `x` and possibly returns a `y`, otherwise it returns Nothing.
Serialize :: x -> Maybe y
```

Let's say I have discovered that this call will never fail. I strengthen
my precondition to make sure that it returns a y, always.

```hs
-- This means the serialize function takes an `x` and always returns a `y`.
Serialize :: x -> y
```

What happens to previous callers who assumed they had to check that y
could fail?

What happens if I weaken a condition?

```hs
-- This means the serialize function takes an `x` and always returns a `y`.
Serialize :: x -> y
```

I thought the caller would need to provide an `x`, but I later on
provided a default `y` which can be returned if `x` is not provided.

```hs
-- This means the serialize function can take an `x` and always returns a `y`.
Serialize :: Maybe x -> y
```

What happens to callers that had previously provided a Maybe type?

It depends on how the language is implemented. If types are checked at compile time, there may be compilation errors. If you have a dynamically typed language, the code will run, but there will be some unnecessary work. 

## In protobuf3, the `required` keyword for schemas was removed. More context here: [Required is removed from protobuf 3](https://stackoverflow.com/a/31814967). In your own words, why do you think it was removed? What are the pros and cons of having a "required" field for backwards and forwards compatibility?

It seems like “required” was removed because it wasn’t well suited for maintainability. What is considered “required” can easily change over time, which will force the developers to modify all of their schemas manually. Additionally, adding / deleting required fields seems to break forward / backward compatibility.
 
The only benefit I can think of is that using a required field makes it very clear to a developer that a certain field is required. 
