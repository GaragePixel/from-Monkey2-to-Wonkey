# Ｔｅｍｐｌａｔｅ－ｃｏｎｓｔｒａｉｎｅｄ　ｏｖｅｒｌｏａｄ:exclamation:
[This is related to the return type overloading researches](https://discord.com/channels/796336780302876683/796338396003172352/1390164016201990205)

```monkey
    Function Avr<P>:Byte( bools:Stack<Bool> ) Where P=Byte
``` **Features:**
- We can use `P` (`Prefix`) instead of `T`.
- We can use the 16 primitive types to overload the return type variable. 
**Limitations:**
- We can't use a type generic `T` for changing `Bool` to anything else and constraint it: `where T=Bool` 
- We can't  overload the argument's type
- We can't use `const name`, `enum name` or `member class name` to override the type of `P` in the constraint. 
```monkey
'16 prefixes for 'template specialization' (static return-type dispatch by constraint)
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

On the 16 possible primitive types, choose the signed type first since unsigned induces a micro overhead.
```monkey
Const _pTrue_:Byte
Const _pFalse_:Short
```Then, implement an expert-level version of the function for your api:
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
```It's only examples. Of course you can write an Untruth-called version of the function. But with only one version called, you can dynamically set the prefix and eliminate every tests.
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

Everything works fine:```monkey
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
```Output:
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

**Considerate this code: **
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
End ```

Unit test:
```monkey
Function Main()
    Local v01:Int=F<_p0_>(New T1)         'prefx,        arg type,         return type,         ok
    Local v02:Int=F<_p0_>(New T2)         'sme prefx,     differ arg tpe,     same return type,     ok
'    Local v03:Float=F<_p0_>(New T1)     'sme prefx,     same arg tpe,     differ1 return type,     fails
'    Local v04:Float=F<_p0_>(New T2)     'sme prefx,     same arg tpe,     differ1 return type,     fails
    Local v05:Int=F<_p1_>(New T1)        'diff prefx,    same arg tpe,     same return type,     ok
    Local v06:Int=F<_p1_>(New T2)        'diff prefx,    differ arg tpe,     same return type,     ok
    Local v07:Float=F<_p2_>(New T1)    'diff prefx,    same arg tpe,    differ2 Return type,     ok
    Local v08:Float=F<_p2_>(New T2)    'diff prefx,    differ arg tpe,    differ2 Return type,     ok
    Local v09:Double=F<_p3_>(New T2)    'diff prefx,    differ arg tpe,    differ3 Return type,     ok
    Local v10:Double=F<_p3_>(New T3)    'diff prefx,    differ arg tpe,    differ3 Return type,     ok
    Local v11:Byte=F<_p3_>(New T3)        'diff prefx,    differ arg tpe,    differ3 Return type,     ok
    Local v12:=F<_p1_,T3,Byte>(New T3)    'diff prefx,    tmpltd arg tpe,    differ3 Return tpe,             ok
    Local v13:=F<_p1_,T1,Int>(New T1)    'diff prefx,    differ templatd arg type,    diff3 Return tpe,     ok
    Local v14:=F<_p2_,T3,Byte>(New T3)    'diff prefx,    differ templatd arg type,    diff3 Return tpe,     ok
    Local v15:=F<_p2_,T1,Int>(New T1)    'diff prefx,    differ templatd arg type,    diff3 Return tpe,     ok
    Local v16:=F<_p2_,T1,Int>(New T1,0)    'diff prefx,    differ templatd arg type,    diff3 Return tpe,     ok

    Local v17:=Ft<Int>(New T1)            'Templt, expl return type
    Local v18:=Ft(New T1,0)                'Templt, impl return type
    Local v19:=Ft(New T1,0,0.0)            'Templt, expl return type with overload
    Local v20:=Ft(New T1,0,0.0,"")        'Templt, expl return type with overload and optional overload 
```
- Overloads with the same prefix and different return types are not allowed. The template resolution requires unique function signatures for a given prefix and argument type combination.
- Using a different prefix (the leading template parameters) allows overloading with different return types and argument types.
- Templated functions can forward arguments and types to lower-level overloads for code reuse and abstraction.
- Optional and pseudo-variadic arguments can be combined with templating, greatly expanding overload flexibility. 

For example, we can write a function who adds 2 angles in a friendly manner, and let the api decides which calculation and datatype should be used according the current choosen angle format within the library, without any branches. 

`_p1_`,` _p2_`... are called **primitive prefixes**. They are used to define `_pTrue_` etc.

In stdlib, the `primative prefixes` are enumerated as a natural sequence from the least frequently used primitive type to the most used ones, avoiding collisions. 16 possible prefixes for a combo:
```monkey
' Prefix's natural sequence
Alias _p0_:TypeInfo,    _p1_:DeclInfo
Alias _p2_:CString,     _p3_:String
Alias _p4_:UShort,      _p5_:UInt
Alias _p6_:ULong,       _p7_:Long
Alias _p8_:Short,       _p9_:Double
Alias _p10_:Byte,       _p11_:UByte
Alias _p12_:Int,        _p13_:Float
Alias _p14_:Object,     _p15_:Variant 
```
This appendix concludes [this topic](https://discord.com/channels/796336780302876683/796338396003172352/1391643077650812938).

