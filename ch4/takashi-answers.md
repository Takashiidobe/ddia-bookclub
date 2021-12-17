# Chapter 4 Questions

## The first concern we have this chapter is forward and backwards compatibility. Talk about a time when you appreciated forwards compatibility, and a time when you appreciated backwards compatibility. What are some of the constraints put on application writers when forward or backwards compatibility is required? What are some of the ways that this can create errors for the user if not respected?

I respect forward compatibility in HTML. Since browsers might be running
a really old version of HTML, the spec says to treat any tag as
invisible. Thus, if I use a tag that isn't supported, it will be
ignored. This has to be done because upgrades have to be backwards and
forwards compatible for the web, since any additions have to not have
any effect on older versions, and must work correctly on newer versions.

Consider backwards compatibility: In C/C++, that means that ABI must be
respected. Thus, new changes to the language must not change the layout
of existing structs/classes/functions, outside of adding to the end of
them. This level of backwards compatibility is noble, but very
difficult: library writers have to be very careful to only add to the
end of structs/classes and can never remove items.

For forwards compatibility, think about how old systems must accept
input it might not understand. In this case, it could disregard
everything it doesn't understand (say v1 of an API takes this schema:

```json
{
  "x": 10
}
```

And now there's a change to the schema, so it takes two parameters:

```json
{
  "x": 10,
  "y": 20
}
```

The previous version could only care about the key "x" and disregard
every other key.
)

## What is the difference between JSON/XML and binary encodings? What are the pros and cons of JSON/XML vs Binary encoding? How do all the formats guarantee forward and backwards compatibility?

JSON and XML differ from binary encodings in that they're not space
efficient -- i.e. they take up more space since they're not packed.
However, they're more human readable. As well, XML and JSON support
fewer data types, especially with regard to numbers. Binary encodings
should be favored due to this.

## How is Avro different from protobuf and Thrift? How does it guarantee forward and backwards compatibility?

Protobuf and Thrift support forward and backwards compatibility thusly:

1. Tag numbers are generated to point to each field.
2. To conserve forward compatibility, only optional fields can be
   removed.
3. To preserve backward compatibility, only optional fields or fields
   with a default can be added.
4. Tag numbers can never be reused.

Avro tackles this by splitting up the schema it uses into two:

Writers and readers can define their own schema, and they simply have to
be compatible:

When data is decoded, the writer's schema and reader's schema is used to
resolve differences. Order doesn't matter because all fields have a
unique name, which is keyed into for both schemas.

As well, if the reader finds a value that is in the writer's schema but
not the reader's, it's ignored. If the code reading expects a value, but
the writer's schema doesn't contain said value, it is filled in with a
default declared by the reader's schema.

To maintain compatibility, you may only add or remove a field that has a
default value. As well, types in Avro are non-nullable, and must be
specified as null. As well, branches can be added to union types
backwards compatibly, but not forward compatibly.

## What are some challenges that occur when serialization and deserialization are done on data stored in a database? In web applications? In Mobile apps? In desktop apps? What are some of the challenges you've faced on making schemas forwards and backwards compatible in production?

Some challenges I've faced have been the following:

For Libraries that are linked against, making sure to establish ABI
compatibility (If ABI is broken, then backwards and forwards compatibility is
broken).

For Web APIs: All changes must always be backwards compatible, since
clients calling the API will always expect a stable API. To combat this,
our team used RPC style versions: so all requests would provide a
version in the query parameter, and the code would use that version to
display different data to the client. We would still need to keep
versions backwards compatible, but it would be easier to display in the
code.

In Mobile Apps, forwards and backwards compatibility must also be
respected, since it can take a long time before some users update their
app (or never, given that they cannot because of OS update problems).

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

### What happens to previous callers who assumed they had to check that y could fail?

Code to previous callers now fails because they do compile time checking
to verify types.

In a language with dynamic types, nothing happens, just that the
previous version of the code would do a redundant check at runtime.

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

### What happens to callers that had previously provided a Maybe type?

For a language with static types, another compile time error, and the
code will not run.

For a language with dynamic types, there is a useless default value that
is provided who's branch is never taken.

## In protobuf3, the `required` keyword for schemas was removed. More context here: [Required is removed from protobuf 3](https://stackoverflow.com/a/31814967). In your own words, why do you think it was removed? What are the pros and cons of having a "required" field for backwards and forwards compatibility?

The `required` keyword makes sense to label data that is always
required, but it breaks backwards and forward compatibility, so it makes
sense to remove it as a footgun for application developers. As well,
what is "required" might change over time, so it's a good usability
change in my opinion.
