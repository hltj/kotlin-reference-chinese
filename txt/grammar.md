# Notation  

This section informally explains the grammar notation used below.  

## Symbols and naming  

_Terminal symbol_ names start with an uppercase letter, e.g. **SimpleName**.  
_Nonterminal symbol_ names start with lowercase letter, e.g. **kotlinFile**.  
Each _production_ starts with a colon (**:**).  
_Symbol definitions_ may have many productions and are terminated by a semicolon (**;**).  
Symbol definitions may be prepended with _attributes_, e.g. `start` attribute denotes a start symbol.  

## EBNF expressions  

Operator `|` denotes _alternative_.  
Operator `*` denotes _iteration_ (zero or more).  
Operator `+` denotes _iteration_ (one or more).  
Operator `?` denotes _option_ (zero or one).  
alpha`{ `beta`}` denotes a nonempty _beta_-separated list of _alpha_'s.   
Operator `++` means that no space or comment allowed between operands.  

# Semicolons  

Kotlin provides `"semicolon inference"`: syntactically, subsentences (e.g., statements, declarations etc) are separated by the pseudo-token [SEMI](#SEMI), which stands for `"semicolon or newline"`. In most cases, there's no need for semicolons in Kotlin code.  

# Syntax  

Relevant pages: [Packages](packages.html)  

start  
kotlinFile  

  : [preamble](#preamble) [topLevelObject](#topLevelObject)*  
  ;  

start  
script  

  : [preamble](#preamble) [expression](#expression)*  
  ;  

##### preamble  
 (used by [script](#script), [kotlinFile](#kotlinFile))  

  : [fileAnnotations](#fileAnnotations)? [packageHeader](#packageHeader)? [import](#import)*  
  ;  

##### fileAnnotations  
 (used by [preamble](#preamble))  

  : [fileAnnotation](#fileAnnotation)*  
  ;  

##### fileAnnotation  
 (used by [fileAnnotations](#fileAnnotations))  

  : `"@"` `"file"` `":"` (`"["` [unescapedAnnotation](#unescapedAnnotation)+ `"]"` | [unescapedAnnotation](#unescapedAnnotation))  
  ;  

##### packageHeader  
 (used by [preamble](#preamble))  

  : [modifiers](#modifiers) `"package"` [SimpleName](#SimpleName){ `"."`} [SEMI](#SEMI)?  
  ;  

See [Packages](packages.html)  

##### import  
 (used by [preamble](#preamble))  

  : `"import"` [SimpleName](#SimpleName){ `"."`} (`"."` `"*"` | `"as"` [SimpleName](#SimpleName))? [SEMI](#SEMI)?  
  ;  

See [Imports](packages.html#imports)  

##### topLevelObject  
 (used by [kotlinFile](#kotlinFile))  

  : [class](#class)  
  : [object](#object)  
  : [function](#function)  
  : [property](#property)  
  : [typeAlias](#typeAlias)  
  ;  

##### typeAlias  
 (used by [memberDeclaration](#memberDeclaration), [declaration](#declaration), [topLevelObject](#topLevelObject))  

  : [modifiers](#modifiers) `"typealias"` [SimpleName](#SimpleName) [typeParameters](#typeParameters)? `"="` [type](#type)  
  ;  

## Classes  

See [Classes and Inheritance](classes.html)  

##### class  
 (used by [memberDeclaration](#memberDeclaration), [declaration](#declaration), [topLevelObject](#topLevelObject))  

  : [modifiers](#modifiers) (`"class"` | `"interface"`) [SimpleName](#SimpleName)  
      [typeParameters](#typeParameters)?  
      [primaryConstructor](#primaryConstructor)?  
 (`":"` [annotations](#annotations) [delegationSpecifier](#delegationSpecifier){ `","`})?  
      [typeConstraints](#typeConstraints)  
 ([classBody](#classBody)? | [enumClassBody](#enumClassBody))  
  ;  

##### primaryConstructor  
 (used by [class](#class), [object](#object))  

  : ([modifiers](#modifiers) `"constructor"`)? (`"("` [functionParameter](#functionParameter){ `","`} `")"`)  
  ;  

##### classBody  
 (used by [objectLiteral](#objectLiteral), [enumEntry](#enumEntry), [class](#class), [companionObject](#companionObject), [object](#object))  

  : (`"{"` [members](#members) `"}"`)?  
  ;  

##### members  
 (used by [enumClassBody](#enumClassBody), [classBody](#classBody))  

  : [memberDeclaration](#memberDeclaration)*  
  ;  

##### delegationSpecifier  
 (used by [objectLiteral](#objectLiteral), [class](#class), [companionObject](#companionObject), [object](#object))  

  : [constructorInvocation](#constructorInvocation)   
  : [userType](#userType)  
  : [explicitDelegation](#explicitDelegation)  
  ;  

##### explicitDelegation  
 (used by [delegationSpecifier](#delegationSpecifier))  

  : [userType](#userType) `"by"` [expression](#expression)   
  ;  

##### typeParameters  
 (used by [typeAlias](#typeAlias), [class](#class), [property](#property), [function](#function))  

  : `"<"` [typeParameter](#typeParameter){ `","`} `">"`  
  ;  

##### typeParameter  
 (used by [typeParameters](#typeParameters))  

  : [modifiers](#modifiers) [SimpleName](#SimpleName) (`":"` [userType](#userType))?  
  ;  

See [Generic classes](generics.html)  

##### typeConstraints  
 (used by [class](#class), [property](#property), [function](#function))  

  : (`"where"` [typeConstraint](#typeConstraint){ `","`})?  
  ;  

##### typeConstraint  
 (used by [typeConstraints](#typeConstraints))  

  : [annotations](#annotations) [SimpleName](#SimpleName) `":"` [type](#type)  
  ;  

See [Generic constraints](generics.html#generic-constraints)  

### Class members  

##### memberDeclaration  
 (used by [members](#members))  

  : [companionObject](#companionObject)  
  : [object](#object)  
  : [function](#function)  
  : [property](#property)  
  : [class](#class)  
  : [typeAlias](#typeAlias)  
  : [anonymousInitializer](#anonymousInitializer)  
  : [secondaryConstructor](#secondaryConstructor)  
  ;  

##### anonymousInitializer  
 (used by [memberDeclaration](#memberDeclaration))  

  : `"init"` [block](#block)  
  ;  

##### companionObject  
 (used by [memberDeclaration](#memberDeclaration))  

  : [modifiers](#modifiers) `"companion"` `"object"` [SimpleName](#SimpleName)? (`":"` [delegationSpecifier](#delegationSpecifier){ `","`})? [classBody](#classBody)?  
  ;  

##### valueParameters  
 (used by [secondaryConstructor](#secondaryConstructor), [function](#function))  

  : `"("` [functionParameter](#functionParameter){ `","`}? `")"`  
  ;  

##### functionParameter  
 (used by [valueParameters](#valueParameters), [primaryConstructor](#primaryConstructor))  

  : [modifiers](#modifiers) (`"val"` | `"var"`)? [parameter](#parameter) (`"="` [expression](#expression))?  
  ;  

##### block  
 (used by [catchBlock](#catchBlock), [anonymousInitializer](#anonymousInitializer), [secondaryConstructor](#secondaryConstructor), [functionBody](#functionBody),[controlStructureBody](#controlStructureBody), [try](#try), [finallyBlock](#finallyBlock))  

  : `"{"` [statements](#statements) `"}"`  
  ;  

##### function  
 (used by [memberDeclaration](#memberDeclaration), [declaration](#declaration), [topLevelObject](#topLevelObject))  

  : [modifiers](#modifiers) `"fun"`  
      [typeParameters](#typeParameters)?  
 ([type](#type) `"."`)?  
      [SimpleName](#SimpleName)  
      [typeParameters](#typeParameters)? [valueParameters](#valueParameters) (`":"` [type](#type))?  
      [typeConstraints](#typeConstraints)  
      [functionBody](#functionBody)?  
  ;  

##### functionBody  
 (used by [getter](#getter), [setter](#setter), [function](#function))  

  : [block](#block)  
  : `"="` [expression](#expression)  
  ;  

##### variableDeclarationEntry  
 (used by [for](#for), [lambdaParameter](#lambdaParameter), [property](#property), [multipleVariableDeclarations](#multipleVariableDeclarations))  

  : [SimpleName](#SimpleName) (`":"` [type](#type))?  
  ;  

##### multipleVariableDeclarations  
 (used by [for](#for), [lambdaParameter](#lambdaParameter), [property](#property))  

  : `"("` [variableDeclarationEntry](#variableDeclarationEntry){ `","`} `")"`  
  ;  

##### property  
 (used by [memberDeclaration](#memberDeclaration), [declaration](#declaration), [topLevelObject](#topLevelObject))  

  : [modifiers](#modifiers) (`"val"` | `"var"`)  
      [typeParameters](#typeParameters)?  
 ([type](#type) `"."`)?  
      ([multipleVariableDeclarations](#multipleVariableDeclarations) | [variableDeclarationEntry](#variableDeclarationEntry))  
      [typeConstraints](#typeConstraints)  
 (`"by"` | `"="` [expression](#expression) [SEMI](#SEMI)?)?  
      ([getter](#getter)? [setter](#setter)? | [setter](#setter)? [getter](#getter)?) [SEMI](#SEMI)?  
  ;  

See [Properties and Fields](properties.html)  

##### getter  
 (used by [property](#property))  

  : [modifiers](#modifiers) `"get"`  
  : [modifiers](#modifiers) `"get"` `"("` `")"` (`":"` [type](#type))? [functionBody](#functionBody)  
  ;  

##### setter  
 (used by [property](#property))  

  : [modifiers](#modifiers) `"set"`  
  : [modifiers](#modifiers) `"set"` `"("` [modifiers](#modifiers) ([SimpleName](#SimpleName) | [parameter](#parameter)) `")"` [functionBody](#functionBody)  
  ;  

##### parameter  
 (used by [functionType](#functionType), [setter](#setter), [functionParameter](#functionParameter))  

  : [SimpleName](#SimpleName) `":"` [type](#type)  
  ;  

##### object  
 (used by [memberDeclaration](#memberDeclaration), [declaration](#declaration), [topLevelObject](#topLevelObject))  

  : `"object"` [SimpleName](#SimpleName) [primaryConstructor](#primaryConstructor)? (`":"` [delegationSpecifier](#delegationSpecifier){ `","`})? [classBody](#classBody)?  

##### secondaryConstructor  
 (used by [memberDeclaration](#memberDeclaration))  

  : [modifiers](#modifiers) `"constructor"` [valueParameters](#valueParameters) (`":"` [constructorDelegationCall](#constructorDelegationCall))? [block](#block)  
  ;  

##### constructorDelegationCall  
 (used by [secondaryConstructor](#secondaryConstructor))  

  : `"this"` [valueArguments](#valueArguments)  
  : `"super"` [valueArguments](#valueArguments)  
  ;  

See [Object expressions and Declarations](object-declarations.html)  

### Enum classes  

See [Enum classes](enum-classes.html)  

##### enumClassBody  
 (used by [class](#class))  

  : `"{"` [enumEntries](#enumEntries) (`";"` [members](#members))? `"}"`  
  ;  

##### enumEntries  
 (used by [enumClassBody](#enumClassBody))  

  : ([enumEntry](#enumEntry){ `","`} `","`? `";"`?)?  
  ;  

##### enumEntry  
 (used by [enumEntries](#enumEntries))  

  : [modifiers](#modifiers) [SimpleName](#SimpleName) (`"("` [arguments](#arguments) `")"`)? [classBody](#classBody)?  
  ;  

## Types  

See [Types](basic-types.html)  

##### type  
 (used by [namedInfix](#namedInfix), [simpleUserType](#simpleUserType), [getter](#getter), [atomicExpression](#atomicExpression), [whenCondition](#whenCondition), [property](#property),[typeArguments](#typeArguments), [function](#function), [typeAlias](#typeAlias), [parameter](#parameter), [functionType](#functionType), [variableDeclarationEntry](#variableDeclarationEntry),[lambdaParameter](#lambdaParameter), [typeConstraint](#typeConstraint))  

  : [typeModifiers](#typeModifiers) [typeReference](#typeReference)  
  ;  

##### typeReference  
 (used by [typeReference](#typeReference), [nullableType](#nullableType), [type](#type))  

  : `"("` [typeReference](#typeReference) `")"`  
  : [functionType](#functionType)  
  : [userType](#userType)  
  : [nullableType](#nullableType)  
  : `"dynamic"`  
  ;  

##### nullableType  
 (used by [typeReference](#typeReference))  

  : [typeReference](#typeReference) `"?"`  
  ;  

##### userType  
 (used by [typeParameter](#typeParameter), [catchBlock](#catchBlock), [callableReference](#callableReference), [typeReference](#typeReference), [delegationSpecifier](#delegationSpecifier),[constructorInvocation](#constructorInvocation), [explicitDelegation](#explicitDelegation))  

  : [simpleUserType](#simpleUserType){ `"."`}  
  ;  

##### simpleUserType  
 (used by [userType](#userType))  

  : [SimpleName](#SimpleName) (`"<"` ([optionalProjection](#optionalProjection) [type](#type) | `"*"`){ `","`} `">"`)?  
  ;  

##### optionalProjection  
 (used by [simpleUserType](#simpleUserType))  

  : [varianceAnnotation](#varianceAnnotation)  
  ;  

##### functionType  
 (used by [typeReference](#typeReference))  

  : ([type](#type) `"."`)? `"("` [parameter](#parameter){ `","`}? `")"` `"->"` [type](#type)?  
  ;  

## Control structures  

See [Control structures](control-flow.html)  

##### controlStructureBody  
 (used by [whenEntry](#whenEntry), [for](#for), [if](#if), [doWhile](#doWhile), [while](#while))  

  : [block](#block)  
  : [blockLevelExpression](#blockLevelExpression)  
  ;  

##### if  
 (used by [atomicExpression](#atomicExpression))  

  : `"if"` `"("` [expression](#expression) `")"` [controlStructureBody](#controlStructureBody) [SEMI](#SEMI)? (`"else"` [controlStructureBody](#controlStructureBody))?  
  ;  

##### try  
 (used by [atomicExpression](#atomicExpression))  

  : `"try"` [block](#block) [catchBlock](#catchBlock)* [finallyBlock](#finallyBlock)?  
  ;  

##### catchBlock  
 (used by [try](#try))  

  : `"catch"` `"("` [annotations](#annotations) [SimpleName](#SimpleName) `":"` [userType](#userType) `")"` [block](#block)  
  ;  

##### finallyBlock  
 (used by [try](#try))  

  : `"finally"` [block](#block)  
  ;  

##### loop  
 (used by [atomicExpression](#atomicExpression))  

  : [for](#for)  
  : [while](#while)  
  : [doWhile](#doWhile)  
  ;  

##### for  
 (used by [loop](#loop))  

  : `"for"` `"("` [annotations](#annotations) ([multipleVariableDeclarations](#multipleVariableDeclarations) | [variableDeclarationEntry](#variableDeclarationEntry)) `"in"` [expression](#expression) `")"` [controlStructureBody](#controlStructureBody)  
  ;  

##### while  
 (used by [loop](#loop))  

  : `"while"` `"("` [expression](#expression) `")"` [controlStructureBody](#controlStructureBody)  
  ;  

##### doWhile  
 (used by [loop](#loop))  

  : `"do"` [controlStructureBody](#controlStructureBody) `"while"` `"("` [expression](#expression) `")"`  
  ;  

## Expressions  

### Precedence  

| Precedence | Title | Symbols |  
| --- | --- | --- |  
| Highest | Postfix | `++`, `--`, `.`, `?.`, `?` |  
 Prefix | `-`, `+`, `++`, `--`, `!`, [`labelDefinition`](#IDENTIFIER)`@` |  
 Type RHS | `:`, `as`, `as?` |  
 Multiplicative | `*`, `/`, `%` |  
 Additive | `+`, `-` |  
 Range | `..` |  
 Infix function | [`SimpleName`](#SimpleName) |  
 Elvis | `?:` |  
 Named checks | `in`, `!in`, `is`, `!is` |  
 Comparison | `<`, `>`, `<=`, `>=` |  
 Equality | `==`, `\!==` |  
 Conjunction | `&&` |  
 Disjunction | `||` |  
| Lowest | Assignment | `=`, `+=`, `-=`, `*=`, `/=`, `%=` |  

### Rules  

##### expression  
 (used by [for](#for), [atomicExpression](#atomicExpression), [longTemplate](#longTemplate), [whenCondition](#whenCondition), [functionBody](#functionBody), [doWhile](#doWhile),[property](#property), [script](#script), [explicitDelegation](#explicitDelegation), [jump](#jump), [while](#while), [arrayAccess](#arrayAccess), [blockLevelExpression](#blockLevelExpression), [if](#if),[when](#when), [valueArguments](#valueArguments), [functionParameter](#functionParameter))  

  : [disjunction](#disjunction) ([assignmentOperator](#assignmentOperator) [disjunction](#disjunction))*  
  ;  

##### disjunction  
 (used by [expression](#expression))  

  : [conjunction](#conjunction) (`"||"` [conjunction](#conjunction))*  
  ;  

##### conjunction  
 (used by [disjunction](#disjunction))  

  : [equalityComparison](#equalityComparison) (`"&&"` [equalityComparison](#equalityComparison))*  
  ;  

##### equalityComparison  
 (used by [conjunction](#conjunction))  

  : [comparison](#comparison) ([equalityOperation](#equalityOperation) [comparison](#comparison))*  
  ;  

##### comparison  
 (used by [equalityComparison](#equalityComparison))  

  : [namedInfix](#namedInfix) ([comparisonOperation](#comparisonOperation) [namedInfix](#namedInfix))*  
  ;  

##### namedInfix  
 (used by [comparison](#comparison))  

  : [elvisExpression](#elvisExpression) ([inOperation](#inOperation) [elvisExpression](#elvisExpression))*  
  : [elvisExpression](#elvisExpression) ([isOperation](#isOperation) [type](#type))?  
  ;  

##### elvisExpression  
 (used by [namedInfix](#namedInfix))  

  : [infixFunctionCall](#infixFunctionCall) (`"?:"` [infixFunctionCall](#infixFunctionCall))*  
  ;  

##### infixFunctionCall  
 (used by [elvisExpression](#elvisExpression))  

  : [rangeExpression](#rangeExpression) ([SimpleName](#SimpleName) [rangeExpression](#rangeExpression))*  
  ;  

##### rangeExpression  
 (used by [infixFunctionCall](#infixFunctionCall))  

  : [additiveExpression](#additiveExpression) (`".."` [additiveExpression](#additiveExpression))*  
  ;  

##### additiveExpression  
 (used by [rangeExpression](#rangeExpression))  

  : [multiplicativeExpression](#multiplicativeExpression) ([additiveOperation](#additiveOperation) [multiplicativeExpression](#multiplicativeExpression))*  
  ;  

##### multiplicativeExpression  
 (used by [additiveExpression](#additiveExpression))  

  : [typeRHS](#typeRHS) ([multiplicativeOperation](#multiplicativeOperation) [typeRHS](#typeRHS))*  
  ;  

##### typeRHS  
 (used by [multiplicativeExpression](#multiplicativeExpression))  

  : [prefixUnaryExpression](#prefixUnaryExpression) ([typeOperation](#typeOperation) [prefixUnaryExpression](#prefixUnaryExpression))*  
  ;  

##### prefixUnaryExpression  
 (used by [typeRHS](#typeRHS))  

  : [prefixUnaryOperation](#prefixUnaryOperation)* [postfixUnaryExpression](#postfixUnaryExpression)  
  ;  

##### postfixUnaryExpression  
 (used by [prefixUnaryExpression](#prefixUnaryExpression), [postfixUnaryOperation](#postfixUnaryOperation))  

  : [atomicExpression](#atomicExpression) [postfixUnaryOperation](#postfixUnaryOperation)*  
  : [callableReference](#callableReference) [postfixUnaryOperation](#postfixUnaryOperation)*  
  ;  

##### callableReference  
 (used by [postfixUnaryExpression](#postfixUnaryExpression))  

  : ([userType](#userType) `"?"`*)? `"::"` [SimpleName](#SimpleName) [typeArguments](#typeArguments)?  
  ;  

##### atomicExpression  
 (used by [postfixUnaryExpression](#postfixUnaryExpression))  

  : `"("` [expression](#expression) `")"`  
  : [literalConstant](#literalConstant)  
  : [functionLiteral](#functionLiteral)  
  : `"this"` [labelReference](#labelReference)?  
  : `"super"` (`"<"` [type](#type) `">"`)? [labelReference](#labelReference)?  
  : [if](#if)  
  : [when](#when)  
  : [try](#try)  
  : [objectLiteral](#objectLiteral)  
  : [jump](#jump)  
  : [loop](#loop)  
  : [collectionLiteral](#collectionLiteral)  
  : [SimpleName](#SimpleName)  
  ;  

##### labelReference  
 (used by [atomicExpression](#atomicExpression), [jump](#jump))  

  : `"@"` ++ [LabelName](#LabelName)  
  ;  

##### labelDefinition  
 (used by [prefixUnaryOperation](#prefixUnaryOperation), [annotatedLambda](#annotatedLambda))  

  : [LabelName](#LabelName) ++ `"@"`  
  ;  

##### literalConstant  
 (used by [atomicExpression](#atomicExpression))  

  : `"true"` | `"false"`  
  : [stringTemplate](#stringTemplate)  
  : [NoEscapeString](#NoEscapeString)  
  : [IntegerLiteral](#IntegerLiteral)  
  : [HexadecimalLiteral](#HexadecimalLiteral)  
  : [CharacterLiteral](#CharacterLiteral)  
  : [FloatLiteral](#FloatLiteral)  
  : `"null"`  
  ;  

##### stringTemplate  
 (used by [literalConstant](#literalConstant))  

  : `"\"``" [stringTemplateElement](#stringTemplateElement)* "`\""  
  ;  

##### stringTemplateElement  
 (used by [stringTemplate](#stringTemplate))  

  : [RegularStringPart](#RegularStringPart)  
  : [ShortTemplateEntryStart](#ShortTemplateEntryStart) ([SimpleName](#SimpleName) | `"this"`)  
  : [EscapeSequence](#EscapeSequence)  
  : [longTemplate](#longTemplate)  
  ;  

##### longTemplate  
 (used by [stringTemplateElement](#stringTemplateElement))  

  : `"${"` [expression](#expression) `"}"`  
  ;  

##### declaration  
 (used by [statement](#statement))  

  : [function](#function)  
  : [property](#property)  
  : [class](#class)  
  : [typeAlias](#typeAlias)  
  : [object](#object)  
  ;  

##### statement  
 (used by [statements](#statements))  

  : [declaration](#declaration)  
  : [blockLevelExpression](#blockLevelExpression)  
  ;  

##### blockLevelExpression  
 (used by [statement](#statement), [controlStructureBody](#controlStructureBody))  

  : [annotations](#annotations) (`"\n"`)+ [expression](#expression)  
  ;  

##### multiplicativeOperation  
 (used by [multiplicativeExpression](#multiplicativeExpression))  

  : `"*"` : `"/"` : `"%"`  
  ;  

##### additiveOperation  
 (used by [additiveExpression](#additiveExpression))  

  : `"+"` : `"-"`  
  ;  

##### inOperation  
 (used by [namedInfix](#namedInfix))  

  : `"in"` : `"!in"`  
  ;  

##### typeOperation  
 (used by [typeRHS](#typeRHS))  

  : `"as"` : `"as?"` : `":"`  
  ;  

##### isOperation  
 (used by [namedInfix](#namedInfix))  

  : `"is"` : `"!is"`  
  ;  

##### comparisonOperation  
 (used by [comparison](#comparison))  

  : `"<"` : `">"` : `">="` : `"<="`  
  ;  

##### equalityOperation  
 (used by [equalityComparison](#equalityComparison))  

  : `"!="` : `"=="`  
  ;  

##### assignmentOperator  
 (used by [expression](#expression))  

  : `"="`  
  : `"+="` : `"-="` : `"*="` : `"/="` : `"%="`  
  ;  

##### prefixUnaryOperation  
 (used by [prefixUnaryExpression](#prefixUnaryExpression))  

  : `"-"` : `"+"`  
  : `"++"` : `"--"`  
  : `"!"`  
  : [annotations](#annotations)  
  : [labelDefinition](#labelDefinition)  
  ;  

##### postfixUnaryOperation  
 (used by [postfixUnaryExpression](#postfixUnaryExpression))  

  : `"++"` : `"--"` : `"!!"`  
  : [callSuffix](#callSuffix)  
  : [arrayAccess](#arrayAccess)  
  : [memberAccessOperation](#memberAccessOperation) [postfixUnaryExpression](#postfixUnaryExpression)   
  ;  

##### callSuffix  
 (used by [constructorInvocation](#constructorInvocation), [postfixUnaryOperation](#postfixUnaryOperation))  

  : [typeArguments](#typeArguments)? [valueArguments](#valueArguments) [annotatedLambda](#annotatedLambda)  
  : [typeArguments](#typeArguments) [annotatedLambda](#annotatedLambda)  
  ;  

##### annotatedLambda  
 (used by [callSuffix](#callSuffix))  

  : (`"@"` [unescapedAnnotation](#unescapedAnnotation))* [labelDefinition](#labelDefinition)? [functionLiteral](#functionLiteral)  

##### memberAccessOperation  
 (used by [postfixUnaryOperation](#postfixUnaryOperation))  

  : `"."` : `"?."` : `"?"`  
  ;  

##### typeArguments  
 (used by [callSuffix](#callSuffix), [callableReference](#callableReference), [unescapedAnnotation](#unescapedAnnotation))  

  : `"<"` [type](#type){ `","`} `">"`  
  ;  

##### valueArguments  
 (used by [callSuffix](#callSuffix), [constructorDelegationCall](#constructorDelegationCall), [unescapedAnnotation](#unescapedAnnotation))  

  : `"("` ([SimpleName](#SimpleName) `"="`)? `"*"`? [expression](#expression){ `","`} `")"`  
  ;  

##### jump  
 (used by [atomicExpression](#atomicExpression))  

  : `"throw"` [expression](#expression)  
  : `"return"` ++ [labelReference](#labelReference)? [expression](#expression)?  
  : `"continue"` ++ [labelReference](#labelReference)?  
  : `"break"` ++ [labelReference](#labelReference)?  
  ;  

##### functionLiteral  
 (used by [atomicExpression](#atomicExpression), [annotatedLambda](#annotatedLambda))  

  : `"{"` [statements](#statements) `"}"`  
  : `"{"` [lambdaParameter](#lambdaParameter){ `","`} `"->"` [statements](#statements) `"}"`  
  ;  

##### lambdaParameter  
 (used by [functionLiteral](#functionLiteral))  

  : [variableDeclarationEntry](#variableDeclarationEntry)  
  : [multipleVariableDeclarations](#multipleVariableDeclarations) (`":"` [type](#type))?  
  ;  

##### statements  
 (used by [block](#block), [functionLiteral](#functionLiteral))  

  : [SEMI](#SEMI)* [statement](#statement){[SEMI](#SEMI)+} [SEMI](#SEMI)*  
  ;  

##### constructorInvocation  
 (used by [delegationSpecifier](#delegationSpecifier))  

  : [userType](#userType) [callSuffix](#callSuffix)  
  ;  

##### arrayAccess  
 (used by [postfixUnaryOperation](#postfixUnaryOperation))  

  : `"["` [expression](#expression){ `","`} `"]"`  
  ;  

##### objectLiteral  
 (used by [atomicExpression](#atomicExpression))  

  : `"object"` (`":"` [delegationSpecifier](#delegationSpecifier){ `","`})? [classBody](#classBody)  
  ;  

##### collectionLiteral  
 (used by [atomicExpression](#atomicExpression))  

  : `"["` [element](#element){ `","`}? `"]"`  
  ;  

#### When-expression  

See [When-expression](control-flow.html#when-expression)  

##### when  
 (used by [atomicExpression](#atomicExpression))  

  : `"when"` (`"("` [expression](#expression) `")"`)? `"{"`  
        [whenEntry](#whenEntry)*  
    `"}"`  
  ;  

##### whenEntry  
 (used by [when](#when))  

  : [whenCondition](#whenCondition){ `","`} `"->"` [controlStructureBody](#controlStructureBody) [SEMI](#SEMI)  
  : `"else"` `"->"` [controlStructureBody](#controlStructureBody) [SEMI](#SEMI)  
  ;  

##### whenCondition  
 (used by [whenEntry](#whenEntry))  

  : [expression](#expression)  
  : (`"in"` | `"!in"`) [expression](#expression)  
  : (`"is"` | `"!is"`) [type](#type)  
  ;  

## Modifiers  

##### modifiers  
 (used by [typeParameter](#typeParameter), [getter](#getter), [packageHeader](#packageHeader), [class](#class), [property](#property), [function](#function), [typeAlias](#typeAlias),[secondaryConstructor](#secondaryConstructor), [enumEntry](#enumEntry), [setter](#setter), [companionObject](#companionObject), [primaryConstructor](#primaryConstructor),[functionParameter](#functionParameter))  

  : ([modifier](#modifier) | [annotations](#annotations))*  
  ;  

##### typeModifiers  
 (used by [type](#type))  

  : ([suspendModifier](#suspendModifier) | [annotations](#annotations))*  
  ;  

##### modifier  
 (used by [modifiers](#modifiers))  

  : [classModifier](#classModifier)  
  : [accessModifier](#accessModifier)  
  : [varianceAnnotation](#varianceAnnotation)  
  : [memberModifier](#memberModifier)  
  : [parameterModifier](#parameterModifier)  
  : [typeParameterModifier](#typeParameterModifier)  
  : [functionModifier](#functionModifier)  
  : [propertyModifier](#propertyModifier)  
  ;  

##### classModifier  
 (used by [modifier](#modifier))  

  : `"abstract"`  
  : `"final"`  
  : `"enum"`  
  : `"open"`  
  : `"annotation"`  
  : `"sealed"`  
  : `"data"`  
  ;  

##### memberModifier  
 (used by [modifier](#modifier))  

  : `"override"`  
  : `"open"`  
  : `"final"`  
  : `"abstract"`  
  : `"lateinit"`  
  ;  

##### accessModifier  
 (used by [modifier](#modifier))  

  : `"private"`  
  : `"protected"`  
  : `"public"`  
  : `"internal"`  
  ;  

##### varianceAnnotation  
 (used by [modifier](#modifier), [optionalProjection](#optionalProjection))  

  : `"in"`  
  : `"out"`  
  ;  

##### parameterModifier  
 (used by [modifier](#modifier))  

  : `"noinline"`  
  : `"crossinline"`  
  : `"vararg"`  
  ;  

##### typeParameterModifier  
 (used by [modifier](#modifier))  

  : `"reified"`  
  ;  

##### functionModifier  
 (used by [modifier](#modifier))  

  : `"tailrec"`  
  : `"operator"`  
  : `"infix"`  
  : `"inline"`  
  : `"external"`  
  : [suspendModifier](#suspendModifier)  
  ;  

##### propertyModifier  
 (used by [modifier](#modifier))  

  : `"const"`  
  ;  

##### suspendModifier  
 (used by [typeModifiers](#typeModifiers), [functionModifier](#functionModifier))  

  : `"suspend"`  
  ;  

## Annotations  

##### annotations  
 (used by [catchBlock](#catchBlock), [prefixUnaryOperation](#prefixUnaryOperation), [blockLevelExpression](#blockLevelExpression), [for](#for), [typeModifiers](#typeModifiers), [class](#class),[modifiers](#modifiers), [typeConstraint](#typeConstraint))  

  : ([annotation](#annotation) | [annotationList](#annotationList))*  
  ;  

##### annotation  
 (used by [annotations](#annotations))  

  : `"@"` ([annotationUseSiteTarget](#annotationUseSiteTarget) `":"`)? [unescapedAnnotation](#unescapedAnnotation)  
  ;  

##### annotationList  
 (used by [annotations](#annotations))  

  : `"@"` ([annotationUseSiteTarget](#annotationUseSiteTarget) `":"`)? `"["` [unescapedAnnotation](#unescapedAnnotation)+ `"]"`  
  ;  

##### annotationUseSiteTarget  
 (used by [annotation](#annotation), [annotationList](#annotationList))  

  : `"field"`  
  : `"file"`  
  : `"property"`  
  : `"get"`  
  : `"set"`  
  : `"receiver"`  
  : `"param"`  
  : `"setparam"`  
  : `"delegate"`  
  ;  

##### unescapedAnnotation  
 (used by [annotation](#annotation), [fileAnnotation](#fileAnnotation), [annotatedLambda](#annotatedLambda), [annotationList](#annotationList))  

  : [SimpleName](#SimpleName){ `"."`} [typeArguments](#typeArguments)? [valueArguments](#valueArguments)?  
  ;  

# Lexical structure  

helper  
##### Digit  
 (used by [IntegerLiteral](#IntegerLiteral), [HexDigit](#HexDigit))  

  : [`"0"`..`"9"`];  

##### IntegerLiteral  
 (used by [literalConstant](#literalConstant))  

  : [Digit](#Digit) ([Digit](#Digit) | `"_"`)*  

##### FloatLiteral  
 (used by [literalConstant](#literalConstant))  

  : \<Java double literal\>;  

helper  
##### HexDigit  
 (used by [RegularStringPart](#RegularStringPart), [HexadecimalLiteral](#HexadecimalLiteral))  

  : [Digit](#Digit) | [`"A"`..`"F"`, `"a"`..`"f"`];  

##### HexadecimalLiteral  
 (used by [literalConstant](#literalConstant))  

  : `"0x"` [HexDigit](#HexDigit) ([HexDigit](#HexDigit) | `"_"`)*;  

##### CharacterLiteral  
 (used by [literalConstant](#literalConstant))  

  : \<character as in Java\>;  

See [Basic types](basic-types.html)  

##### NoEscapeString  
 (used by [literalConstant](#literalConstant))  

  : \<"""-quoted string\>;  

##### RegularStringPart  
 (used by [stringTemplateElement](#stringTemplateElement))  

  : \<any character other than backslash, quote, $ or newline\>  
[ShortTemplateEntryStart](#ShortTemplateEntryStart):  
  : `"$"`  
[EscapeSequence](#EscapeSequence):  
  : [UnicodeEscapeSequence](#UnicodeEscapeSequence) | [RegularEscapeSequence](#RegularEscapeSequence)  
[UnicodeEscapeSequence](#UnicodeEscapeSequence):  
  : `"\u"` [HexDigit](#HexDigit){4}  
[RegularEscapeSequence](#RegularEscapeSequence):  
  : `"\"` \<any character other than newline\>  

See [String templates](basic-types.html#templates)  

##### SEMI  
 (used by [whenEntry](#whenEntry), [if](#if), [statements](#statements), [packageHeader](#packageHeader), [property](#property), [import](#import))  

  : \<semicolon or newline\>;  

##### SimpleName  
 (used by [typeParameter](#typeParameter), [catchBlock](#catchBlock), [simpleUserType](#simpleUserType), [atomicExpression](#atomicExpression), [LabelName](#LabelName),[packageHeader](#packageHeader), [class](#class), [object](#object), [infixFunctionCall](#infixFunctionCall), [function](#function), [typeAlias](#typeAlias), [parameter](#parameter),[callableReference](#callableReference), [variableDeclarationEntry](#variableDeclarationEntry), [stringTemplateElement](#stringTemplateElement), [enumEntry](#enumEntry), [setter](#setter),[import](#import), [companionObject](#companionObject), [valueArguments](#valueArguments), [unescapedAnnotation](#unescapedAnnotation), [typeConstraint](#typeConstraint))  

  : \<java identifier\>  
  : ```"`"``` \<java identifier\> ```"`"```  
  ;  

See [Java interoperability](java-interop.html)  

##### LabelName  
 (used by [labelReference](#labelReference), [labelDefinition](#labelDefinition))  

  : `"@"` [SimpleName](#SimpleName);  

See [Returns and jumps](returns.html)  
