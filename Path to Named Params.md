# The way to achieve *Named Parameter*: 
## The tuple - fixed number parameters, possible inference via variant
```monkey
Alias Args:Tuple2<String,String>

Function fn<Tuple>(args:Args)
End

Function Main()
    fn(New Args("title: test",""))
End
```
In fact, this technique works very well with fixed arguments, it's fast and the same TArgs can be used for any combination of primitive type arguments. This static, compile-time fixed approach paves the way for a future dynamic approach: passing a single string parameter containing key-value pairs, which can be parsed and reflected to simulate real named arguments, optionally parsed using reflection, or using a string-variant pair approach (Map<String,Variant>). This is a good foundation for dynamic named arguments.

I prefer a parser, which is much slower than the mapping strategy, but it avoids the use of idiomatic constructors. After all, if you're looking for performance, this language already has all the assets to make it superior to 'virtual machine languages'. But if you're looking for convenience and performance isn't your goal, argument parsing can help you write more readable code. Anyway, the approach presented here is very interesting for clear and maintainable multi-argument APIs.
