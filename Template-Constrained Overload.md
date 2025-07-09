# template-constrained overload

## Multitype Return overloading
This was something Sibly wanted to do. He'd written about it on his blog, about **multitype return overloading**.
So I did some research to make sure it was what I'd thought it was.
And I discovered right away that it's not something many languages, even recent ones, can do. When it's possible, the language quickly gets talked about and becomes the best in the world.
But what I discovered later is that the compiler actually knows how to do it very well, and it's not something @d_a_n_i_l_o or @.seyha added. So there you have it, you can, for example, do this:
```monkey
Function A:Int(a:Int)
  Return a
End 
Function A:String(a:String)
  Return a
End

Function Main()
  Local aint:=A(10)
  Local astring:=A("10")
    
  Print aint+1     'should output: 11
  Print astring+1  'should output: 101
End
```

EDIT: This is not the true return type overloading.
The true overloading will returns contextually a type while the argument is from the same type.
So I will do some further tests later for making a new todo entry or not.

So, about the return type overloading, [from this discussion](https://discord.com/channels/796336780302876683/796338396003172352/1390061676161929338) we can't achieve it idiomatically.
```monkey
Function a:Int(b:Int)
    Return b
End 
Function a:Float(c:Int)
    Return c
End 
Function Main()
    Local aInt:Int=a(10) 'should returns 10
    Local aFloat:Float=a(10) 'should returns 10.0 
End
```
This will signal a duplicate declaration. Therefore, it's not a true return type overload.
We can use a tuple. For example, `Tuple2<Variant,TypeInfo>` or, in very specific cases, `Tuple2<Variant,Bool>` (for example).
We can cheat with generic programming:
```monkey
Function Main()
    Local aInt:=a<Int>(10) 'should returns 10
    Local aFloat:=a<Float>(10) 'should returns 10.0 
End
Function a<T>:T(c:Int)
    Return c
End
```
But without cheating, we can also use [TVar](https://discord.com/channels/796336780302876683/796338396003172352/1390115267102900380). This will need testing to be sure, but I think using TVar should be a bit faster than the Tuple2. Let's imagine that TVar can act like a `Tuple2<Variant,TypeInfo>` for a specific purpose.

[This is related to the return type overloading researches](https://discord.com/channels/796336780302876683/796338396003172352/1390164016201990205)

```monkey
    Function Avr<P>:Byte( bools:Stack<Bool> ) Where P=Byte
```
**Features:**
- We can use `P` (`Prefix`) instead of `T`.
- We can use the 12 primitive types to overload the return type variable. 
**Limitations:**
- We can't use a type generic `T` for changing `Bool` to anything else and constraint it: `where T=Bool` 
- We can't  overload the argument's type
- We can't use `const name`, `enum name` or `member class name` to override the type of `P` in the constraint. 
```monkey
'12 prefixes for 'template specialization' (static return-type dispatch by constraint)
Alias _pPct_:Byte
Alias _pFract_:Short
```
We can overload 1 wire here. `P`.
It results that if you take these functions: 
```monkey
Function Avr<P>:Byte( bools:Stack<Bool> ) Where P=Byte '_pPct_
    Local s:UInt=0
    For Local n:=Eachin bools
        s+=UInt(n)*100
    End
    Return s/bools.Length
End 

Function Avr<P>:Float( bools:Stack<Bool> ) Where P=Short '_pFract_
    Local s:Float=0
    For Local n:=Eachin bools
        s+=n
    End
    Return s/bools.Length
End 
```
And then call them in this way:
```monkey
    Local myStackOfBool:=New Stack<Bool>(New Bool[](True,True,False))
    Print Avr<_pPct_>(myStackOfBool)+"%"
    Print Avr<_pFract_>(myStackOfBool)
```
It will outputs:
```
66%
0.666666687
```
It's static, 𝘴ƥᴇᴇᴅ oғ lɪɢʜᴛ.
### Application: This workaround will allows to write eventually functions like:
```monkey
Function AddAngle<_pDeg_>:Byte16(…)
Function AddAngle<_pRad_>:Float16(…)
```
- *Byte16, Float16… are in-coming datatypes*
- *Use `p` to indicate the `const` used as prefix*

Then call them like this:
```
AddAngle<AngleMode>(…)
```
Where `AngleMode` a dynamic parameter.

On the 12 possible primitive types, choose the signed type first since unsigned induces a micro overhead.
```monkey
Const _pTrue_:Byte
Const _pFalse_:Short
```
Then, implement an expert-level version of the function for your api:
```monkey
Function Truth<P>:Int( bools:Stack<Bool> ) Where P=Byte '_pTrue_
    Local s:Int=0
    For Local n:=Eachin bools
        s+=n
    End
    Return s
End 

Function Truth<P>:Int( bools:Stack<Bool> ) Where P=Short '_pFalse_
    Local s:Int=0
    For Local n:=Eachin bools
        s+=Not n
    End
    Return s
End 
```
It's only examples. Of course you can write an Untruth-called version of the function. But with only one version called, you can dynamically set the prefix and eliminate every tests.
And if the api is planned to be used at the normal/pro-level, use overloading (implements a branch):
```monkey
Function Truth:Int( bools:Stack<Bool>, untruth:Bool=False )
    Local s:Int=0
    If untruth
        For Local n:=Eachin bools
            s+=Not n
        End
    Else
        For Local n:=Eachin bools
            s+=n
        End
    End 
    Return s
End
```

Everything works fine:
```monkey
#Import "<stdlib>"
Using stdlib.aliases.prefixes 'template-constrained overload's most used aliases 
                              'now a part of stdlib
Function Main()
    Local bools:=New Stack<Bool>(New Bool[](True,True,False))
    Print Avr<_pPct_>(bools)+"%"
    Print Avr<_pFract_>(bools)
    Print "Truth (TCO): "+Truth<_pTruth_>(bools)
    Print "Untruth (TCO): "+Truth<_pUntruth_>(bools)
    Print "Truth (uses preset overload): "+Truth(bools)
    Print "Truth (explicitly overloaded): "+Truth(bools,False) '<- default: deactivated 
    Print "Untruth (explicitly overloaded): "+Truth(bools,True) '<- untruth activated
End
```
Output:
```
66%
0.666666687
Truth (TCO): 2
Untruth (TCO): 1
Truth (uses preset overload): 2
Truth (explicitly overloaded): 2
Untruth (explicitly overloaded): 1
```
This isn't a true return type overload from the Sibly's todo list, of course, but it works perfectly in some specific cases and optimizes the library, which is necessary, for example, for AI applications. By using many micro-optimizations at the API implementation's low-level, we can achieve considerable speed while keeping the API easy to use for hobbyists. 

This work was related to the [return type overloading researches](https://discord.com/channels/796336780302876683/796338396003172352/1390164016201990205) from the Sibly's [todo list](https://monkeycoder.co.nz/monkey2-roadmap/) (Return type based overloading) 
[The problem is analyzed here](https://discord.com/channels/796336780302876683/1391944347154518026/1391945538571599942).

## Appendix: 

**Considerate this code:**
```monkey
Class T1
End 
Class T2
End 
Class T3
End 

' different argument types

Function F<P>:Int(t:T1) Where P=_p0_
    Return Null
End

Function F<P>:Int(t:T2) Where P=_p0_
    Return Null
End

' different returned types with the same prefix (do not works)

'Function F<P>:Float(t:T1) Where P=_p0_
'    Return Null
'End
'
'Function F<P>:Float(t:T2) Where P=_p0_
    'Return Null
'End

' different returned types with different prefix

Function F<P>:Int(t:T1) Where P=_p1_
    Return Null
End

Function F<P>:Int(t:T2) Where P=_p1_
    Return Null
End

Function F<P>:Float(t:T1) Where P=_p2_
    Return Null
End

Function F<P>:Float(t:T2) Where P=_p2_
    Return Null
End

Function F<P>:Double(t:T2) Where P=_p3_
    Return Null
End

Function F<P>:Double(t:T3) Where P=_p3_
    Return Null
End

Function F<P,C,T>:T(t:C) Where P=_p1_
    Return Cast<T>(0)
End

Function F<P,C,T>:T(t:C) Where P=_p2_
    Return Cast<T>(0)
End

Function F<P,C,T>:T(t1:C,t2:Int) Where P=_p2_
    Return Cast<T>(0)
End

Function F<P,C,T>:T(t1:C) Where P=_p2_ And T=Float
    Return Cast<T>(0)
End

'Templating:

Function Ft<R>:R(t1:T1) 'templating
    Return F<_p1_,T1,R>(t1)
End 

Function Ft<T1>:Float(t1:T1) 'templating
    Return F<_p1_,T1,Float>(t1)
End 

Function Ft<T1>:Float(t1:T1,t2:Int) 'templating with overloaded argument
    Return F<_p1_,T1,Float>(t1)
End 

Function Ft<T1>:Float(t1:T1,t2:Int,t3:Float,t4:String="") 'templating with optional overloaded argument
    Return F<_p1_,T1,Float>(t1)
End
```
Unit test:
```monkey
Function Main()
    Local v01:Int=F<_p0_>(New T1)         'prefx,         arg type,                    return type,                 ok
    Local v02:Int=F<_p0_>(New T2)         'sme prefx,     differ arg tpe,              same return type,            ok
'    Local v03:Float=F<_p0_>(New T1)      'sme prefx,     same arg tpe,                differ1 return type,         fails
'    Local v04:Float=F<_p0_>(New T2)      'sme prefx,     same arg tpe,                differ1 return type,         fails
    Local v05:Int=F<_p1_>(New T1)         'diff prefx,    same arg tpe,                same return type,            ok
    Local v06:Int=F<_p1_>(New T2)         'diff prefx,    differ arg tpe,              same return type,            ok
    Local v07:Float=F<_p2_>(New T1)       'diff prefx,    same arg tpe,                differ2 Return type,         ok
    Local v08:Float=F<_p2_>(New T2)       'diff prefx,    differ arg tpe,              differ2 Return type,         ok
    Local v09:Double=F<_p3_>(New T2)      'diff prefx,    differ arg tpe,              differ3 Return type,         ok
    Local v10:Double=F<_p3_>(New T3)      'diff prefx,    differ arg tpe,              differ3 Return type,         ok
    Local v11:Byte=F<_p3_>(New T3)        'diff prefx,    differ arg tpe,              differ3 Return type,         ok
    Local v12:=F<_p1_,T3,Byte>(New T3)    'diff prefx,    tmpltd arg tpe,              differ3 Return tpe,          ok
    Local v13:=F<_p1_,T1,Int>(New T1)     'diff prefx,    differ templatd arg type,    diff3 Return tpe,            ok
    Local v14:=F<_p2_,T3,Byte>(New T3)    'diff prefx,    differ templatd arg type,    diff3 Return tpe,            ok
    Local v15:=F<_p2_,T1,Int>(New T1)     'diff prefx,    differ templatd arg type,    diff3 Return tpe,            ok
    Local v16:=F<_p2_,T1,Int>(New T1,0)   'diff prefx,    differ templatd arg type,    diff3 Return tpe,            ok

    Local v17:=Ft<Int>(New T1)            'Templt, expl return type
    Local v18:=Ft(New T1,0)               'Templt, impl return type
    Local v19:=Ft(New T1,0,0.0)           'Templt, expl return type with overload
    Local v20:=Ft(New T1,0,0.0,"")        'Templt, expl return type with overload and optional overload

    'Usage examples:

    Local bools:=New Stack<Bool>(New Bool[](True,True,False))
    Print "Avr<_pFastPct>: "+Avr<_pFastPct_>(bools)+"%"
    Print "Avr<_pPct_>: "+Avr<_pPct_>(bools)+"%"
    Print "Avr<_pFract_>: "+Avr<_pFract_>(bools)
    Print "Truth (TCO): "+Truth<_pTruth_>(bools) 'output 2
    Print "Untruth (TCO): "+Truth<_pUntruth_>(bools) 'output 1
    Print "Truth (uses preset overload): "+Truth(bools) 'output 2
    Print "Truth (explicitly overloaded): "+Truth(bools,False) '<- default: deactivated 'output 2
    Print "Untruth (explicitly overloaded): "+Truth(bools,True) '<- untruth activated 'output 1
```
- Overloads with the same prefix and different return types are not allowed. The template resolution requires unique function signatures for a given prefix and argument type combination.
- Using a different prefix (the leading template parameters) allows overloading with different return types and argument types.
- Templated functions can forward arguments and types to lower-level overloads for code reuse and abstraction.
- Optional and pseudo-variadic arguments can be combined with templating, greatly expanding overload flexibility. 

For example, we can write a function who adds 2 angles in a friendly manner, and let the api decides which calculation and datatype should be used according the current choosen angle format within the library, without any branches. 

`_p1_`,` _p2_`... are called **primitive prefixes**. They are used to define `_pTrue_` etc.

In stdlib, the `primative prefixes` are enumerated as a natural sequence from the least frequently used primitive type to the most used ones, avoiding collisions. 12 possible prefixes for a combo:
```monkey
' Prefix's natural sequence
Alias _p0_:UShort,	_p1_:UInt
Alias _p2_:ULong,	_p3_:Long
Alias _p4_:Short,	_p5_:Double
Alias _p6_:Byte,	_p7_:UByte
Alias _p8_:Int,		_p9_:Float
Alias _p10_:Object, 	_p11_:Variant
```
This appendix concludes [this topic](https://discord.com/channels/796336780302876683/796338396003172352/1391643077650812938).

# [Template-constrained overload](https://discord.com/channels/796336780302876683/796338396003172352/1390164016201990205)
This is related to the Sibly's [todo list](https://monkeycoder.co.nz/monkey2-roadmap/). On the topic and its [alternative for the actual compiler](https://discord.com/channels/796336780302876683/796338396003172352/1391643077650812938), I discute about the actual problem: the **return type overloading**.

Let's see how the Monkey2's compiler handles functions:
```
┎─────────────┒ Local in:Inx     Argument's type ↴
┃     Inx     ┃ Local myOutx:Outx=MyFunction(in:Inx) ← The Call
┖─────────────┚               ↑
       ↓                      ╘══════╕ returned type
┎─────────────┒                      │
┃     Inx     ┃ Function MyFunction:Outx(in:Inx) ← Implementation
┃     Outx    ┃ 
┖─────────────┚ If Outx = Assignx, pass the value directly
┊Type Checking┊ Elseif Outx can be casted to Assignx, cast, return, ok
┎─────────────┒ Else, compile error
┃   Assignx   ┃ 
┖─────────────┚
```
If the same named function exists as:
```
┎─────────────┒┎─────────────┒
┃     Inx     ┃┃     Iny     ┃ 
┃     Outx    ┃┃     Outx    ┃ 
┖─────────────┚┖─────────────┚
```
You can call them with the appropriated type of in:
```
┎─────────────┒┎─────────────┒
┃     Inx     ┃┃     Iny     ┃ The calls
┖─────────────┚┖─────────────┚
       ↓              ↓
┎─────────────┒┎─────────────┒
┃     Inx     ┃┃     Iny     ┃ The implementations:
┃     Outx    ┃┃     Outx    ┃ The function is overloaded with the In type.
┖─────────────┚┖─────────────┚ Note that the returned type must be identical.
┊Type Checking┊┊Type Checking┊
┎─────────────┒┎─────────────┒
┃   Assignx   ┃┃   Assignx   ┃ The variable to assigns with the returned result
┖─────────────┚┖─────────────┚ from the function.
With the actual compiler:```
┇   We can't  ┇┇    But we   ┇ The second column demonstrates 
┇      do     ┇┇    can do   ┇ the 'Template-constrained overload' technique.
┎─────────────┒┎─────────────┒
┃     Inx     ┃┃     Inx     ┃
┖─────────────┚┖─────────────┚
       ↓              ↓
┎─────────────┒┎─────────────┒
┃     Inx     ┃┃     Inx     ┃ since z is not implemented,
┃    Outx/y   ┃┃     Outz    ┃ it's casted by type checking.
┖─────────────┚┖─────────────┚
       ❌      ┊Type Checking┊ strongly typed, the technique
┎─────────────┒┎─────────────┒ works by type checking at compile-time.
┃  Assignx/y  ┃┃   Assignz   ┃
┖─────────────┚┖─────────────┚
```
The 1st column wil trig a [duplicate declaration error]( https://discord.com/channels/796336780302876683/796338396003172352/1390164016201990205)
The second column allows [my technique](https://discord.com/channels/796336780302876683/796338396003172352/1391997702228934810) to works.

This is what the actual compiler can't do (the Mark's unachieved [todo list](https://monkeycoder.co.nz/monkey2-roadmap/ )):
```
┇                         Can't do                         ┇
┎─────────────┒┎─────────────┒┎─────────────┒┎─────────────┒
┃     Inx     ┃┃     Iny     ┃┃    Inx/y    ┃┃    Inx/y    ┃ 'x/y means 'generic'
┃    Outx/y   ┃┃    Outx/y   ┃┃     Outx    ┃┃     Outy    ┃
┖─────────────┚┖─────────────┚┖─────────────┚┖─────────────┚
       ↓              ↓              ↓              ↓
┎─────────────┒┎─────────────┒┎─────────────┒┎─────────────┒
┃     Inx     ┃┃     Iny     ┃┃    Inx/y    ┃┃    Inx/y    ┃
┃    Outx/y   ┃┃    Outx/y   ┃┃     Outx    ┃┃     Outy    ┃
┖─────────────┚┖─────────────┚┖─────────────┚┖─────────────┚
       ║              ║       ┊Type Checking┊┊Type Checking┊
┎─────────────┒┎─────────────┒┎─────────────┒┎─────────────┒
┃  Assignx/y  ┃┃  Assignx/y  ┃┃   Assignz   ┃┃   Assignz   ┃
┖─────────────┚┖─────────────┚┖─────────────┚┖─────────────┚
      ❌             ❌             ❌             ❌
  duplicated        todo            todo           todo
  ```
