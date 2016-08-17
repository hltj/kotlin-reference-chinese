# Notation  

This section informally explains the grammar notation used below.  

## Symbols and naming  

_Terminal symbol_ names start with an uppercase letter, e.g. **SimpleName**.  
_Nonterminal symbol_ names start with lowercase letter, e.g. **kotlinFile**.  
Each _production_ starts with a colon (**:**).  
_Symbol definitions_ may have many productions and are terminated by a semicolon (**;**).  
Symbol definitions may be prepended with _attributes_, e.g. `start` attribute denotes a start symbol.  

## EBNF expressions  

Operator `|` denotes _alternative_.  
Operator `*` denotes _iteration_ (zero or more).  
Operator `+` denotes _iteration_ (one or more).  
Operator `?` denotes _option_ (zero or one).  
alpha`{`beta`}` denotes a nonempty _beta_-separated list of _alpha_’s.   
Operator `++` means that no space or comment allowed between operands.  

# Semicolons  

Kotlin provides “semicolon inference”: syntactically, subsentences (e.g., statements, declarations etc) are separated by the pseudo-token [SEMI](#semi), which stands for “semicolon or newline”. In most cases, there’s no need for semicolons in Kotlin code.  

# Syntax  

Relevant pages: [Packages](packages.html)  

start  
kotlinFile  

  : [preamble](#preamble) [toplevelObject](#toplevelobject)*  
  ;  

start  
script  

  : [preamble](#preamble) [expression](#expression)*  
  ;  

##### preamble  
 (used by [script](#script), [kotlinFile](#kotlinfile))  

  : [fileAnnotations](#fileannotations)? [packageHeader](#packageheader)? [import](#import)*  
  ;  

##### fileAnnotations  
 (used by [preamble](#preamble))  

  : [fileAnnotation](#fileannotation)*  
  ;  

##### fileAnnotation  
 (used by [fileAnnotations](#fileannotations))  

  : `"@"` `"file"` `":"` (`"["` [annotationEntry](#annotationentry)+ `"]"` | [annotationEntry](#annotationentry))  
  ;  

##### packageHeader  
 (used by [preamble](#preamble))  

  : [modifiers](#modifiers) `"package"` [SimpleName](#simplename){ `"."`} [SEMI](#semi)?  
  ;  

##### import  
 (used by [preamble](#preamble), [package](#package))  

  : `"import"` [SimpleName](#simplename){ `"."`} (`"."` `"*"` | `"as"` [SimpleName](#simplename))? [SEMI](#semi)?  
  ;  

See [Imports](packages.html#imports)  

##### toplevelObject  
 (used by [package](#package), [kotlinFile](#kotlinfile))  

  : [package](#package)  
  : [class](#class)  
  : [object](#object)  
  : [function](#function)  
  : [property](#property)  
  ;  

##### package  
 (used by [toplevelObject](#toplevelobject))  

  : `"package"` [SimpleName](#simplename){ `"."`} `"{"`  
       [import](#import)*  
       [toplevelObject](#toplevelobject)*  
    `"}"`  
  ;  

See [Packages](packages.html)  

## Classes  

See [Classes and Inheritance](classes.html)  

##### class  
 (used by [memberDeclaration](#memberdeclaration), [declaration](#declaration), [toplevelObject](#toplevelobject))  

  : [modifiers](#modifiers) (`"class"` | `"interface"`) [SimpleName](#simplename)  
      [typeParameters](#typeparameters)?  
      [primaryConstructor](#primaryconstructor)?  
      (`":"` [annotations](#annotations) [delegationSpecifier](#delegationspecifier){ `","`})?  
      [typeConstraints](#typeconstraints)  
      ([classBody](#classbody)? | [enumClassBody](#enumclassbody))  
  ;  

##### primaryConstructor  
 (used by [class](#class), [object](#object))  

  : ([modifiers](#modifiers) `"constructor"`)? (`"("` [functionParameter](#functionparameter){ `","`} `")"`)  
  ;  

##### classBody  
 (used by [objectLiteral](#objectliteral), [enumEntry](#enumentry), [class](#class), [object](#object))  

  : (`"{"` [members](#members) `"}"`)?  
  ;  

##### members  
 (used by [enumClassBody](#enumclassbody), [classBody](#classbody))  

  : [memberDeclaration](#memberdeclaration)*  
  ;  

##### delegationSpecifier  
 (used by [objectLiteral](#objectliteral), [class](#class), [object](#object))  

  : [constructorInvocation](#constructorinvocation)   
  : [userType](#usertype)  
  : [explicitDelegation](#explicitdelegation)  
  ;  

##### explicitDelegation  
 (used by [delegationSpecifier](#delegationspecifier))  

  : [userType](#usertype) `"by"` [expression](#expression)   
  ;  

##### typeParameters  
 (used by [class](#class), [property](#property), [function](#function))  

  : `"<"` [typeParameter](#typeparameter){ `","`} `">"`  
  ;  

##### typeParameter  
 (used by [typeParameters](#typeparameters))  

  : [modifiers](#modifiers) [SimpleName](#simplename) (`":"` [userType](#usertype))?  
  ;  

See [Generic classes](generics.html)  

##### typeConstraints  
 (used by [class](#class), [property](#property), [function](#function))  

  : (`"where"` [typeConstraint](#typeconstraint){ `","`})?  
  ;  

##### typeConstraint  
 (used by [typeConstraints](#typeconstraints))  

  : [annotations](#annotations) [SimpleName](#simplename) `":"` [type](#type)  
  ;  

See [Generic constraints](generics.html#generic-constraints)  

### Class members  

##### memberDeclaration  
 (used by [members](#members))  

  : [companionObject](#companionobject)  
  : [object](#object)  
  : [function](#function)  
  : [property](#property)  
  : [class](#class)  
  : [typeAlias](#typealias)  
  : [anonymousInitializer](#anonymousinitializer)  
  : [secondaryConstructor](#secondaryconstructor)  
  ;  

##### anonymousInitializer  
 (used by [memberDeclaration](#memberdeclaration))  

  : `"init"` [block](#block)  
  ;  

##### companionObject  
 (used by [memberDeclaration](#memberdeclaration))  

  : [modifiers](#modifiers) `"companion"` `"object"`  
  ;  

##### valueParameters  
 (used by [secondaryConstructor](#secondaryconstructor), [function](#function))  

  : `"("` [functionParameter](#functionparameter){ `","`}? `")"`   
  ;  

##### functionParameter  
 (used by [valueParameters](#valueparameters), [primaryConstructor](#primaryconstructor))  

  : [modifiers](#modifiers) (`"val"` | `"var"`)? [parameter](#parameter) (`"="` [expression](#expression))?  
  ;  

##### initializer  
 (used by [enumEntry](#enumentry))  

  : [annotations](#annotations) [constructorInvocation](#constructorinvocation)   
  ;  

##### block  
 (used by [catchBlock](#catchblock), [anonymousInitializer](#anonymousinitializer), [secondaryConstructor](#secondaryconstructor), [functionBody](#functionbody), [try](#try),[finallyBlock](#finallyblock))  

  : `"{"` [statements](#statements) `"}"`  
  ;  

##### function  
 (used by [memberDeclaration](#memberdeclaration), [declaration](#declaration), [toplevelObject](#toplevelobject))  

  : [modifiers](#modifiers) `"fun"` [typeParameters](#typeparameters)?  
      ([type](#type) `"."` | [annotations](#annotations))?  
      [SimpleName](#simplename)  
      [typeParameters](#typeparameters)? [valueParameters](#valueparameters) (`":"` [type](#type))?  
      [typeConstraints](#typeconstraints)  
      [functionBody](#functionbody)?  
  ;  

##### functionBody  
 (used by [getter](#getter), [setter](#setter), [function](#function))  

  : [block](#block)  
  : `"="` [expression](#expression)  
  ;  

##### variableDeclarationEntry  
 (used by [for](#for), [property](#property), [multipleVariableDeclarations](#multiplevariabledeclarations))  

  : [SimpleName](#simplename) (`":"` [type](#type))?  
  ;  

##### multipleVariableDeclarations  
 (used by [for](#for), [property](#property))  

  : `"("` [variableDeclarationEntry](#variabledeclarationentry){ `","`} `")"`  
  ;  

##### property  
 (used by [memberDeclaration](#memberdeclaration), [declaration](#declaration), [toplevelObject](#toplevelobject))  

  : [modifiers](#modifiers) (`"val"` | `"var"`)  
      [typeParameters](#typeparameters)? ([type](#type) `"."` | [annotations](#annotations))?  
      ([multipleVariableDeclarations](#multiplevariabledeclarations) | [variableDeclarationEntry](#variabledeclarationentry))  
      [typeConstraints](#typeconstraints)  
      (`"by"` | `"="` [expression](#expression) [SEMI](#semi)?)?  
      ([getter](#getter)? [setter](#setter)? | [setter](#setter)? [getter](#getter)?) [SEMI](#semi)?  
  ;  

See [Properties and Fields](properties.html)  

##### getter  
 (used by [property](#property))  

  : [modifiers](#modifiers) `"get"`  
  : [modifiers](#modifiers) `"get"` `"("` `")"` (`":"` [type](#type))? [functionBody](#functionbody)  
  ;  

##### setter  
 (used by [property](#property))  

  : [modifiers](#modifiers) `"set"`  
  : [modifiers](#modifiers) `"set"` `"("` [modifiers](#modifiers) ([SimpleName](#simplename) | [parameter](#parameter)) `")"` [functionBody](#functionbody)  
  ;  

##### parameter  
 (used by [functionType](#functiontype), [setter](#setter), [functionParameter](#functionparameter))  

  : [SimpleName](#simplename) `":"` [type](#type)  
  ;  

##### object  
 (used by [memberDeclaration](#memberdeclaration), [declaration](#declaration), [toplevelObject](#toplevelobject))  

  : `"object"` [SimpleName](#simplename) [primaryConstructor](#primaryconstructor)? (`":"` [delegationSpecifier](#delegationspecifier){ `","`})? [classBody](#classbody)?   

##### secondaryConstructor  
 (used by [memberDeclaration](#memberdeclaration))  

  : [modifiers](#modifiers) `"constructor"` [valueParameters](#valueparameters) (`":"` [constructorDelegationCall](#constructordelegationcall))? [block](#block)  
  ;  

##### constructorDelegationCall  
 (used by [secondaryConstructor](#secondaryconstructor))  

  : `"this"` [valueArguments](#valuearguments)  
  : `"super"` [valueArguments](#valuearguments)  
  ;  

See [Object expressions and Declarations](object-declarations.html)  

### Enum classes[](#enum-classes)  

See [Enum classes](enum-classes.html)  

##### enumClassBody  
 (used by [class](#class))  

  : `"{"` [enumEntries](#enumentries) (`";"` [members](#members))? `"}"`  
  ;  

##### enumEntries  
 (used by [enumClassBody](#enumclassbody))  

  : [enumEntry](#enumentry)*  
  ;  

##### enumEntries  
 (used by [enumClassBody](#enumclassbody))  

  : ([enumEntry](#enumentry) `","`? )?  
  ;  

##### enumEntry  
 (used by [enumEntries](#enumentries))  

  : [modifiers](#modifiers) [SimpleName](#simplename) ((`":"` [initializer](#initializer)) | (`"("` [arguments](#arguments) `")"`))? [classBody](#classbody)?  
  ;  

## Types  

See [Types](basic-types.html)  

##### type  
 (used by [isRHS](#isrhs), [simpleUserType](#simpleusertype), [parameter](#parameter), [functionType](#functiontype), [atomicExpression](#atomicexpression), [getter](#getter),[variableDeclarationEntry](#variabledeclarationentry), [property](#property), [typeArguments](#typearguments), [typeConstraint](#typeconstraint), [function](#function))  

  : [annotations](#annotations) [typeDescriptor](#typedescriptor)  
  ;  

##### typeDescriptor  
 (used by [nullableType](#nullabletype), [typeDescriptor](#typedescriptor), [type](#type))  

  : `"("` [typeDescriptor](#typedescriptor) `")"`  
  : [functionType](#functiontype)  
  : [userType](#usertype)  
  : [nullableType](#nullabletype)  
  : `"dynamic"`  
  ;  

##### nullableType  
 (used by [typeDescriptor](#typedescriptor))  

  : [typeDescriptor](#typedescriptor) `"?"`  

##### userType  
 (used by [typeParameter](#typeparameter), [catchBlock](#catchblock), [callableReference](#callablereference), [typeDescriptor](#typedescriptor),[delegationSpecifier](#delegationspecifier), [constructorInvocation](#constructorinvocation), [explicitDelegation](#explicitdelegation))  

  : (`"package"` `"."`)? [simpleUserType](#simpleusertype){ `"."`}  
  ;  

##### simpleUserType  
 (used by [userType](#usertype))  

  : [SimpleName](#simplename) (`"<"` ([optionalProjection](#optionalprojection) [type](#type) | `"*"`){ `","`} `">"`)?  
  ;  

##### optionalProjection  
 (used by [simpleUserType](#simpleusertype))  

  : [varianceAnnotation](#varianceannotation)  
  ;  

##### functionType  
 (used by [typeDescriptor](#typedescriptor))  

  : ([type](#type) `"."`)? `"("` ([parameter](#parameter) | [modifiers](#modifiers)  [type](#type)){ `","`} `")"` `"->"` [type](#type)?  
  ;  

## Control structures  

See [Control structures](control-flow.html)  

##### if  
 (used by [atomicExpression](#atomicexpression))  

  : `"if"` `"("` [expression](#expression) `")"` [expression](#expression) [SEMI](#semi)? (`"else"` [expression](#expression))?  
  ;  

##### try  
 (used by [atomicExpression](#atomicexpression))  

  : `"try"` [block](#block) [catchBlock](#catchblock)* [finallyBlock](#finallyblock)?  
  ;  

##### catchBlock  
 (used by [try](#try))  

  : `"catch"` `"("` [annotations](#annotations) [SimpleName](#simplename) `":"` [userType](#usertype) `")"` [block](#block)  
  ;  

##### finallyBlock  
 (used by [try](#try))  

  : `"finally"` [block](#block)  
  ;  

##### loop  
 (used by [atomicExpression](#atomicexpression))  

  : [for](#for)  
  : [while](#while)  
  : [doWhile](#dowhile)  
  ;  

##### for  
 (used by [loop](#loop))  

  : `"for"` `"("` [annotations](#annotations) ([multipleVariableDeclarations](#multiplevariabledeclarations) | [variableDeclarationEntry](#variabledeclarationentry)) `"in"` [expression](#expression) `")"` [expression](#expression)  
  ;  

##### while  
 (used by [loop](#loop))  

  : `"while"` `"("` [expression](#expression) `")"` [expression](#expression)  
  ;  

##### doWhile  
 (used by [loop](#loop))  

  : `"do"` [expression](#expression) `"while"` `"("` [expression](#expression) `")"`  
  ;  

## Expressions  

### Precedence  

| Precedence | Title | Symbols |  
| --- | --- | --- |  
| Highest | Postfix | `++`, `--`, `.`, `?.`, `?` |  
 Prefix | `-`, `+`, `++`, `--`, `!`, [`labelDefinition@`](#identifier)`@` |  
 Type RHS | `:`, `as`, `as?` |  
 Multiplicative | `*`, `/`, `%` |  
 Additive | `+`, `-` |  
 Range | `..` |  
 Infix function | [`SimpleName`](#simplename) |  
 Elvis | `?:` |  
 Named checks | `in`, `!in`, `is`, `!is` |  
 Comparison | `<`, `>`, `<=`, `>=` |  
 Equality | `==`, `\!==` |  
 Conjunction | `&&` |  
 Disjunction | `||` |  
| Lowest | Assignment | `=`, `+=`, `-=`, `*=`, `/=`, `%=` |  

### Rules  

##### expression  
 (used by [for](#for), [atomicExpression](#atomicexpression), [longTemplate](#longtemplate), [whenCondition](#whencondition), [functionBody](#functionbody), [doWhile](#dowhile),[property](#property), [script](#script), [explicitDelegation](#explicitdelegation), [jump](#jump), [while](#while), [whenEntry](#whenentry), [arrayAccess](#arrayaccess),[statement](#statement), [if](#if), [when](#when), [valueArguments](#valuearguments), [functionParameter](#functionparameter))  

  : [disjunction](#disjunction) ([assignmentOperator](#assignmentoperator) [disjunction](#disjunction))*  
  ;  

##### disjunction  
 (used by [expression](#expression))  

  : [conjunction](#conjunction) (`"||"` [conjunction](#conjunction))*  
  ;  

##### conjunction  
 (used by [disjunction](#disjunction))  

  : [equalityComparison](#equalitycomparison) (`"&&"` [equalityComparison](#equalitycomparison))*  
  ;  

##### equalityComparison  
 (used by [conjunction](#conjunction))  

  : [comparison](#comparison) ([equalityOperation](#equalityoperation) [comparison](#comparison))*  
  ;  

##### comparison  
 (used by [equalityComparison](#equalitycomparison))  

  : [namedInfix](#namedinfix) ([comparisonOperation](#comparisonoperation) [namedInfix](#namedinfix))*  
  ;  

##### namedInfix  
 (used by [comparison](#comparison))  

  : [elvisExpression](#elvisexpression) ([inOperation](#inoperation) [elvisExpression](#elvisexpression))*  
  : [elvisExpression](#elvisexpression) ([isOperation](#isoperation) [isRHS](#isrhs))?  
  ;  

##### elvisExpression  
 (used by [namedInfix](#namedinfix))  

  : [infixFunctionCall](#infixfunctioncall) (`"?:"` [infixFunctionCall](#infixfunctioncall))*  
  ;  

##### infixFunctionCall  
 (used by [elvisExpression](#elvisexpression))  

  : [rangeExpression](#rangeexpression) ([SimpleName](#simplename) [rangeExpression](#rangeexpression))*  
  ;  

##### rangeExpression  
 (used by [infixFunctionCall](#infixfunctioncall))  

  : [additiveExpression](#additiveexpression) (`".."` [additiveExpression](#additiveexpression))*  
  ;  

##### additiveExpression  
 (used by [rangeExpression](#rangeexpression))  

  : [multiplicativeExpression](#multiplicativeexpression) ([additiveOperation](#additiveoperation) [multiplicativeExpression](#multiplicativeexpression))*  
  ;  

##### multiplicativeExpression  
 (used by [additiveExpression](#additiveexpression))  

  : [typeRHS](#typerhs) ([multiplicativeOperation](#multiplicativeoperation) [typeRHS](#typerhs))*  
  ;  

##### typeRHS  
 (used by [multiplicativeExpression](#multiplicativeexpression))  

  : [prefixUnaryExpression](#prefixunaryexpression) ([typeOperation](#typeoperation) [prefixUnaryExpression](#prefixunaryexpression))*  
  ;  

##### prefixUnaryExpression  
 (used by [typeRHS](#typerhs))  

  : [prefixUnaryOperation](#prefixunaryoperation)* [postfixUnaryExpression](#postfixunaryexpression)  
  ;  

##### postfixUnaryExpression  
 (used by [prefixUnaryExpression](#prefixunaryexpression), [postfixUnaryOperation](#postfixunaryoperation))  

  : [atomicExpression](#atomicexpression) [postfixUnaryOperation](#postfixunaryoperation)*  
  : [callableReference](#callablereference) [postfixUnaryOperation](#postfixunaryoperation)*  
  ;  

##### callableReference  
 (used by [postfixUnaryExpression](#postfixunaryexpression))  

  : ([userType](#usertype) `"?"`*)? `"::"` [SimpleName](#simplename) [typeArguments](#typearguments)?  
  ;  

##### atomicExpression  
 (used by [postfixUnaryExpression](#postfixunaryexpression))  

  : `"("` [expression](#expression) `")"`  
  : [literalConstant](#literalconstant)  
  : [functionLiteral](#functionliteral)  
  : `"this"` [labelReference](#labelreference)?  
  : `"super"` (`"<"` [type](#type) `">"`)? [labelReference](#labelreference)?  
  : [if](#if)  
  : [when](#when)  
  : [try](#try)  
  : [objectLiteral](#objectliteral)  
  : [jump](#jump)  
  : [loop](#loop)  
  : [SimpleName](#simplename)  
  : [FieldName](#fieldname)  
  : `"package"`   
  ;  

##### labelReference  
 (used by [atomicExpression](#atomicexpression), [jump](#jump))  

  : `"@"` ++ [LabelName](#labelname)  
  ;  

##### labelDefinition  
 (used by [prefixUnaryOperation](#prefixunaryoperation), [annotatedLambda](#annotatedlambda))  

  : [LabelName](#labelname) ++ `"@"`  
  ;  

##### literalConstant  
 (used by [atomicExpression](#atomicexpression))  

  : `"true"` | `"false"`  
  : [stringTemplate](#stringtemplate)  
  : [NoEscapeString](#noescapestring)  
  : [IntegerLiteral](#integerliteral)  
  : [HexadecimalLiteral](#hexadecimalliteral)  
  : [CharacterLiteral](#characterliteral)  
  : [FloatLiteral](#floatliteral)  
  : `"null"`  
  ;  

##### stringTemplate  
 (used by [literalConstant](#literalconstant))  

  : `"\"` `"` [stringTemplateElement](#stringtemplateelement)* `"\""`  
  ;  

##### stringTemplateElement  
 (used by [stringTemplate](#stringtemplate))  

  : [RegularStringPart](#regularstringpart)  
  : [ShortTemplateEntryStart](#shorttemplateentrystart) ([SimpleName](#simplename) | `"this"`)  
  : [EscapeSequence](#escapesequence)  
  : [longTemplate](#longtemplate)  
  ;  

##### longTemplate  
 (used by [stringTemplateElement](#stringtemplateelement))  

  : `"${"` [expression](#expression) `"}"`  
  ;  

##### isRHS  
 (used by [namedInfix](#namedinfix), [whenCondition](#whencondition))  

  : [type](#type)  
  ;  

##### declaration  
 (used by [statement](#statement))  

  : [function](#function)  
  : [property](#property)  
  : [class](#class)  
  : [object](#object)  
  ;  

##### statement  
 (used by [statements](#statements))  

  : [declaration](#declaration)  
  : [expression](#expression)  
  ;  

##### multiplicativeOperation  
 (used by [multiplicativeExpression](#multiplicativeexpression))  

  : `"*"` : `"/"` : `"%"`  
  ;  

##### additiveOperation  
 (used by [additiveExpression](#additiveexpression))  

  : `"+"` : `"-"`  
  ;  

##### inOperation  
 (used by [namedInfix](#namedinfix))  

  : `"in"` : `"!in"`  
  ;  

##### typeOperation  
 (used by [typeRHS](#typerhs))  

  : `"as"` : `"as?"` : `":"`  
  ;  

##### isOperation  
 (used by [namedInfix](#namedinfix))  

  : `"is"` : `"!is"`  
  ;  

##### comparisonOperation  
 (used by [comparison](#comparison))  

  : `"<"` : `">"` : `">="` : `"<="`  
  ;  

##### equalityOperation  
 (used by [equalityComparison](#equalitycomparison))  

  : `"!="` : `"=="`  
  ;  

##### assignmentOperator  
 (used by [expression](#expression))  

  : `"="`  
  : `"+="` : `"-="` : `"*="` : `"/="` : `"%="`  
  ;  

##### prefixUnaryOperation  
 (used by [prefixUnaryExpression](#prefixunaryexpression))  

  : `"-"` : `"+"`  
  : `"++"` : `"--"`  
  : `"!"`    
  : [annotations](#annotations)   
  : [labelDefinition](#labeldefinition)  
  ;  

##### postfixUnaryOperation  
 (used by [postfixUnaryExpression](#postfixunaryexpression))  

  : `"++"` : `"--"` : `"!!"`  
  : [callSuffix](#callsuffix)  
  : [arrayAccess](#arrayaccess)  
  : [memberAccessOperation](#memberaccessoperation) [postfixUnaryExpression](#postfixunaryexpression)   
  ;  

##### callSuffix  
 (used by [constructorInvocation](#constructorinvocation), [postfixUnaryOperation](#postfixunaryoperation))  

  : [typeArguments](#typearguments)? [valueArguments](#valuearguments) [annotatedLambda](#annotatedlambda)  
  : [typeArguments](#typearguments) [annotatedLambda](#annotatedlambda)  
  ;  

##### annotatedLambda  
 (used by [callSuffix](#callsuffix))  

  : (`"@"` [annotationEntry](#annotationentry))* [labelDefinition](#labeldefinition)? [functionLiteral](#functionliteral)  

##### memberAccessOperation  
 (used by [postfixUnaryOperation](#postfixunaryoperation))  

  : `"."` : `"?."` : `"?"`  
  ;  

##### typeArguments  
 (used by [callSuffix](#callsuffix), [callableReference](#callablereference), [unescapedAnnotation](#unescapedannotation))  

  : `"<"` [type](#type){ `","`} `">"`  
  ;  

##### valueArguments  
 (used by [callSuffix](#callsuffix), [constructorDelegationCall](#constructordelegationcall), [unescapedAnnotation](#unescapedannotation))  

  : `"("` ([SimpleName](#simplename) `"="`)? `"*"`? [expression](#expression){ `","`} `")"`  
  ;  

##### jump  
 (used by [atomicExpression](#atomicexpression))  

  : `"throw"` [expression](#expression)  
  : `"return"` ++ [labelReference](#labelreference)? [expression](#expression)?  
  : `"continue"` ++ [labelReference](#labelreference)?  
  : `"break"` ++ [labelReference](#labelreference)?  
  ;  

##### functionLiteral  
 (used by [atomicExpression](#atomicexpression), [annotatedLambda](#annotatedlambda))  

  : `"{"` [statements](#statements) `"}"`  
  : `"{"` ([modifiers](#modifiers) [SimpleName](#simplename)){ `","`} `"->"` [statements](#statements) `"}"`  
  ;  

##### statements  
 (used by [block](#block), [functionLiteral](#functionliteral))  

  : [SEMI](#semi)* [statement](#statement){[SEMI](#semi)+} [SEMI](#semi)*  
  ;  

##### constructorInvocation  
 (used by [delegationSpecifier](#delegationspecifier), [initializer](#initializer))  

  : [userType](#usertype) [callSuffix](#callsuffix)  
  ;  

##### arrayAccess  
 (used by [postfixUnaryOperation](#postfixunaryoperation))  

  : `"["` [expression](#expression){ `","`} `"]"`  
  ;  

##### objectLiteral  
 (used by [atomicExpression](#atomicexpression))  

  : `"object"` (`":"` [delegationSpecifier](#delegationspecifier){ `","`})? [classBody](#classbody)   
  ;  

#### Pattern matching  

See [When-expression](control-flow.html#when-expression)  

##### when  
 (used by [atomicExpression](#atomicexpression))  

  : `"when"` (`"("` [expression](#expression) `")"`)? `"{"`  
        [whenEntry](#whenentry)*  
    `"}"`  
  ;  

##### whenEntry  
 (used by [when](#when))  

  : [whenCondition](#whencondition){ `","`} `"->"` [expression](#expression) [SEMI](#semi)  
  : `"else"` `"->"` [expression](#expression) [SEMI](#semi)  
  ;  

##### whenCondition  
 (used by [whenEntry](#whenentry))  

  : [expression](#expression)  
  : (`"in"` | `"!in"`) [expression](#expression)  
  : (`"is"` | `"!is"`) [isRHS](#isrhs)  
  ;  

## Modifiers  

##### modifiers  
 (used by [typeParameter](#typeparameter), [getter](#getter), [packageHeader](#packageheader), [class](#class), [property](#property), [functionLiteral](#functionliteral),[function](#function), [functionType](#functiontype), [secondaryConstructor](#secondaryconstructor), [setter](#setter), [enumEntry](#enumentry), [companionObject](#companionobject),[primaryConstructor](#primaryconstructor), [functionParameter](#functionparameter))  

  : [modifier](#modifier)*  
  ;  

##### modifier  
 (used by [modifiers](#modifiers))  

  : [modifierKeyword](#modifierkeyword)  
  ;  

##### modifierKeyword  
 (used by [modifier](#modifier))  

  : [classModifier](#classmodifier)  
  : [accessModifier](#accessmodifier)  
  : [varianceAnnotation](#varianceannotation)  
  : [memberModifier](#membermodifier)  
  : [annotations](#annotations)  
  ;  

##### classModifier  
 (used by [modifierKeyword](#modifierkeyword))  

  : `"abstract"`  
  : `"final"`  
  : `"enum"`  
  : `"open"`  
  : `"annotation"`  
  ;  

##### memberModifier  
 (used by [modifierKeyword](#modifierkeyword))  

  : `"override"`  
  : `"open"`  
  : `"final"`  
  : `"abstract"`  
  ;  

##### accessModifier  
 (used by [modifierKeyword](#modifierkeyword))  

  : `"private"`  
  : `"protected"`  
  : `"public"`  
  : `"internal"`  
  ;  

##### varianceAnnotation  
 (used by [modifierKeyword](#modifierkeyword), [optionalProjection](#optionalprojection))  

  : `"in"`  
  : `"out"`  
  ;  

## Annotations  

##### annotations  
 (used by [catchBlock](#catchblock), [prefixUnaryOperation](#prefixunaryoperation), [for](#for), [modifierKeyword](#modifierkeyword), [class](#class), [property](#property),[type](#type), [typeConstraint](#typeconstraint), [function](#function), [initializer](#initializer))  

  : ([annotation](#annotation) | [annotationList](#annotationlist))*  
  ;  

##### annotation  
 (used by [annotations](#annotations))  

  : `"@"` ([annotationUseSiteTarget](#annotationusesitetarget) `":"`)? [unescapedAnnotation](#unescapedannotation)  
  ;  

##### annotationList  
 (used by [annotations](#annotations))  

  : `"@"` ([annotationUseSiteTarget](#annotationusesitetarget) `":"`)? `"["` [unescapedAnnotation](#unescapedannotation)+ `"]"`  
  ;  

##### annotationUseSiteTarget  
 (used by [annotation](#annotation), [annotationList](#annotationlist))  

  : `"file"`  
  : `"field"`  
  : `"property"`  
  : `"get"`  
  : `"set"`  
  : `"param"`  
  : `"sparam"`  
  ;  

##### unescapedAnnotation  
 (used by [annotation](#annotation), [annotationList](#annotationlist))  

  : [SimpleName](#simplename){ `"."`} [typeArguments](#typearguments)? [valueArguments](#valuearguments)?  
  ;  

# Lexical structure  

helper  
##### Digit  
 (used by [IntegerLiteral](#integerliteral), [HexDigit](#hexdigit))  

  : [`"0"`..`"9"`];  

##### IntegerLiteral  
 (used by [literalConstant](#literalconstant))  

  : [Digit](#digit)+?  

##### FloatLiteral  
 (used by [literalConstant](#literalconstant))  

  : \<Java double literal\>;  

helper  
##### HexDigit  
 (used by [RegularStringPart](#regularstringpart), [HexadecimalLiteral](#hexadecimalliteral))  

  : [Digit](#digit) | [`"A"`..`"F"`, `"a"`..`"f"`];  

##### HexadecimalLiteral  
 (used by [literalConstant](#literalconstant))  

  : `"0x"` [HexDigit](#hexdigit)+;  

##### CharacterLiteral  
 (used by [literalConstant](#literalconstant))  

  : \<character as in Java\>;  

See [Basic types](basic-types.html)  

##### NoEscapeString  
 (used by [literalConstant](#literalconstant))  

  : \<"""-quoted string\>;  

##### RegularStringPart  
 (used by [stringTemplateElement](#stringtemplateelement))  

  : \<any character other than backslash, quote, $ or newline\>  
[ShortTemplateEntryStart](#shorttemplateentrystart):  
  : `"$"`  
[EscapeSequence](#escapesequence):  
  : [UnicodeEscapeSequence](#unicodeescapesequence) | [RegularEscapeSequence](#regularescapesequence)  
[UnicodeEscapeSequence](#unicodeescapesequence):  
  : `"\u"` [HexDigit](#hexdigit){4}  
[RegularEscapeSequence](#regularescapesequence):  
  : `"\"` \<any character other than newline\>  

See [String templates](basic-types.html#templates)  

##### SEMI  
 (used by [whenEntry](#whenentry), [if](#if), [statements](#statements), [packageHeader](#packageheader), [property](#property), [import](#import))  

  : \<semicolon or newline\>;  

##### SimpleName  
 (used by [typeParameter](#typeparameter), [catchBlock](#catchblock), [simpleUserType](#simpleusertype), [atomicExpression](#atomicexpression), [LabelName](#labelname),[package](#package), [packageHeader](#packageheader), [class](#class), [object](#object), [functionLiteral](#functionliteral), [infixFunctionCall](#infixfunctioncall), [function](#function),[parameter](#parameter), [callableReference](#callablereference), [FieldName](#fieldname), [variableDeclarationEntry](#variabledeclarationentry),[stringTemplateElement](#stringtemplateelement), [setter](#setter), [enumEntry](#enumentry), [import](#import), [valueArguments](#valuearguments),[unescapedAnnotation](#unescapedannotation), [typeConstraint](#typeconstraint))  

  : \<java identifier\>  
  : ```"`"``` \<java identifier\> ```"`"```  
  ;  

See [Java interoperability](java-interop.html)  

##### FieldName  
 (used by [atomicExpression](#atomicexpression))  

  : `"$"` [SimpleName](#simplename);  

##### LabelName  
 (used by [labelReference](#labelreference), [labelDefinition](#labeldefinition))  

  : `"@"` [SimpleName](#simplename);  

See [Returns and jumps](returns.html)  
