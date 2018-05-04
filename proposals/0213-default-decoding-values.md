# Default Values for Decodable Properties

* Proposal: [SE-0213](0213-default-decoding-values.md)
* Authors: [Caleb Kleveter](https://github.com/calebkleveter)
* Review Manager: N/A
* Status: Proposal Review
* Implementation: N/A

## Introduction

This proposal introduces the ability to Swift to synthesize default values for a property of a `Decodable` type if a value for the property is not found in the data being decoded.

## Motivation

The `Decodable` protocol requires a conforming type to implement a `init(form:Decoder)` method. By conforming a type to `Decodable` we can use any `Decoder` implementation to convert `Data` to the `Decodable` type.

While the initializer required by `Decodable` is usually synthesized by Swift, sometimes it is necessary to create a custom implementation of the method. One reason for creating a custom implementation is for default property values. If a value for a property cannot be found in the data being decoded, the decoder with throw an error, and the decoding will stop. If we want a default value for property, we implement the initializer ourselves. The line looks something like this:

```swift
self.active = try container.decode(Bool.self, forKey: .active) ?? true
```

The downside to this is you have to create a whole initializer for the type, when you most likely only need one or two default values.

You might try to set a default value on the property:

```swift
var active: Bool = true
```

But if you do that `active` is always set to `true`, even if `false` is passed in the data.

## Proposed Solution

When we set a default value on a property:

```swift
var active: Bool = true
```

That value should be used as a default decoding value if no value for the property is found in the data.

Instead of synthesizing this for the `active` property:

```swift
self.active = true
```

We should attempt to decode a value first:

```swift
self.active = try container.decode(Bool.self, forKey: .active) ?? true
```

## Impact on existing code

There could be some code bases that rely on the current default implementation. This would cause these implementations to break and they would need to implement a custom initializer.

## Effect on ABI stability

Unknown

## Effect on API resilience

Unknown

## Alternatives considered

To be discussed by the community.
