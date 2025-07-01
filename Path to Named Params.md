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

I prefer a parser, which is much slower than the mapping strategy, but it avoids the use of idiomatic constructors. After all, if you're looking for performance, this language already has all the assets to make it superior to 'virtual machine languages'. But if you're looking for convenience and performance isn't your goal, argument parsing can help you write more readable code. 

Anyway, even if this system is not as flexible as modern implementations, the approach presented here is very interesting for clear and maintainable multi-argument APIs, making the use of aliases a way to write less verbose code is also well used to specify a typing on an object than on a function call.

---

## The embryonic class argument 

For this study, we will only implement a single argument passing, which will allow us to test the casting, write a simple lexer and prepare the implementation of the multiple argument via a container whose choice and use will be discussed.
### This technique uses a hybrid approach: 
```monkey
Function Arg:TArg(pair:String)
    Return New TArg(pair)
End
Class TArg
    Method New(pair:String)
        _pair=pair
    End 
    Method Value<T>:T()
        Local e:=_pair.Split(":")
        Local type:=e[0].Replace(" ","")
        Local strv:=e[1].Replace(" ","")
        Return Cast<T>(strv)
    End 
    Field _pair:String
End 
```Mark wanted to do this using variants. I'm proving here that it's possible to do it with a hybrid parsing method. Hybrid because there aren't really any lexel heavy operations involved as the split doesn't create new strings until the replace method. Hybrid because atomic while  the multiple arguments is handled idiomatically.

```monkey
Function myFunc(arg:Arg)
    Local a:=1+arg.Value<Int>()
    Print a
End

Function Main()
    myFunc(Arg("length: 1"))
End
```
`Output: 2` 

That's not bad for a single-argument version. We can also create a version for any number of arguments; [that was the goal and dream of many people here](https://discord.com/channels/796336780302876683/796359262980800542/1123692821677871254), wasn't it?

In the good ol'days, we had to declare the type during the function call.
Now, we extract the type **after** the function call. This strips the variables of their type and allows us to precede the variable with a nice label, which, in this case, doesn't matter, since the order of the variables must be exactly preserved. Of course, the tradeoffs is the lexing at run time, making this approach as fast as Python :face_with_open_eyes_and_hand_over_mouth:. Using a lexel on an idiomatic implementation is still faster than writing a tokenizer. Yet this is what we need to do to complete an unordered Named Parameter system.

### Multiple arguments via tokenizer

We can go further by imposing a form within the function. The is key for to implement an unordered arguments but at this point, we look for the label name in the form when we parse it into the arguments as a way to infer the type. And IFF we find it then we use the type declared in the container to cast the variable. Because of that, we will first ordered multiple-argument via tokenizer.

Considerating top-bottom strategy, one possible usage example:
```monkey
Function Main()
    myFunc(Args("     title: ~qmyTitle~q, 
                      height: 10, 
                      width: 10.5"))
End
``` 
Possible custom-function using this system:
```monkey
Function myFunc(args:Args)

'│ Call │ Create params │ Define a type array │ Declare the types │ Labelize         
   args.Init(New tParams(New TypeInfo[]( ️  ️  ️  ️ __t_string__,       'title                          ️  ️  ️  ️  ️  ️ ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️   ️  ️  ️__t_int__,          'height                         ️   ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️  ️ __t_float__)))      'width                         
    Local y:=args.Get<Int>("height")
    
    Print y
End
```
However, if I write:
```monkey
    myFunc(Args("     title: ~qmyTitle~q, 
                      height: 10, 
                      width: 10.5"))
```
My first instinct is to expect that what I've declared under these labels can be declared in any order, and also to infer the types. For example, here, 10 is necessarily an integer, and 10.5 is a float32. So there's no point in showing you an implementation. We will speak instead about the container mentionned above.
