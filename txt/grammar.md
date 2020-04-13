# 语法

## Description

### Notation

The notation used on this page corresponds to the ANTLR 4 notation with a few exceptions for better readability:
- omitted lexer rule actions and commands,
- omitted lexical modes.

Short description:
- operator `|` denotes _alternative_,
- operator `*` denotes _iteration_ (zero or more),
- operator `+` denotes _iteration_ (one or more),
- operator `?` denotes _option_ (zero or one),
- operator `..` denotes _range_ (from left to right),
- operator `~` denotes _negation_.

### Grammar source files

Kotlin grammar source files (in ANTLR format) are located in the [Kotlin specification repository](https://github.com/Kotlin/kotlin-spec):
- **[KotlinLexer.g4](https://github.com/Kotlin/kotlin-spec/tree/master/grammar/src/main/antlr/KotlinLexer.g4)** describes [lexical structure](#lexical-grammar);
- **[UnicodeClasses.g4](https://github.com/Kotlin/kotlin-spec/tree/master/grammar/src/main/antlr/UnicodeClasses.g4)** describes the characters that can be used in identifiers (these rules are omitted on this page for better readability);
- **[KotlinParser.g4](https://github.com/Kotlin/kotlin-spec/tree/master/grammar/src/main/antlr/KotlinParser.g4)** describes [syntax](#syntax-grammar).

The grammar on this page corresponds to the grammar files above.

### Symbols and naming

_Terminal symbol_ names start with an uppercase letter, e.g. [Identifier](#Identifier).  
_Non-terminal symbol_ names start with a lowercase letter, e.g. [kotlinFile](#kotlinFile).  

Symbol definitions may be documented with _attributes_:

- `start` attribute denotes a symbol that represents the whole source file (see [kotlinFile](#kotlinFile) and [script](#script)),
- `helper` attribute denotes a lexer fragment rule (used only inside other terminal symbols).

Also for better readability some simplifications are made:
- lexer rules consisting of one string literal element are inlined to the use site,
- new line tokens are excluded (new lines are not allowed in some places, see source grammar files for details).

### Scope

The grammar corresponds to the latest stable version of the Kotlin compiler excluding lexer and parser rules for experimental features that are disabled by default.


## Syntax grammar

### General

Relevant pages: [Packages](packages.html)



start  
<h4 id="kotlinFile">kotlinFile</h4>

 **:**  [shebangLine](#shebangLine)**?**  [fileAnnotation](#fileAnnotation)**\***  [packageHeader](#packageHeader) [importList](#importList) [topLevelObject](#topLevelObject)**\***   `EOF`   
 **;** 


start  
<h4 id="script">script</h4>

 **:**  [shebangLine](#shebangLine)**?**  [fileAnnotation](#fileAnnotation)**\***  [packageHeader](#packageHeader) [importList](#importList) **(** [statement](#statement) [semi](#semi)**)** **\***   `EOF`   
 **;** 

<h4 id="shebangLine">shebangLine</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[kotlinFile](#kotlinFile)<span style="color:gray">, </span>[script](#script)<span style="color:gray">)</span>  
 **:**  [ShebangLine](#ShebangLine)  
 **;** 

<h4 id="fileAnnotation">fileAnnotation</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[kotlinFile](#kotlinFile)<span style="color:gray">, </span>[script](#script)<span style="color:gray">)</span>  
 **:**  [ANNOTATION_USE_SITE_TARGET_FILE](#ANNOTATION_USE_SITE_TARGET_FILE) **(** **(**  `'['`  [unescapedAnnotation](#unescapedAnnotation)**+**   `']'` **)**  **|**  [unescapedAnnotation](#unescapedAnnotation)**)**   
 **;** 

See [Packages](packages.html)


<h4 id="packageHeader">packageHeader</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[kotlinFile](#kotlinFile)<span style="color:gray">, </span>[script](#script)<span style="color:gray">)</span>  
 **:**  **(**  `'package'`  [identifier](#identifier) [semi](#semi)**?** **)** **?**   
 **;** 

See [Imports](packages.html#imports)


<h4 id="importList">importList</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[kotlinFile](#kotlinFile)<span style="color:gray">, </span>[script](#script)<span style="color:gray">)</span>  
 **:**  [importHeader](#importHeader)**\***   
 **;** 

<h4 id="importHeader">importHeader</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[importList](#importList)<span style="color:gray">)</span>  
 **:**   `'import'`  [identifier](#identifier) **(** **(**  `'.'`   `'*'` **)**  **|**  [importAlias](#importAlias)**)** **?**  [semi](#semi)**?**   
 **;** 

<h4 id="importAlias">importAlias</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[importHeader](#importHeader)<span style="color:gray">)</span>  
 **:**   `'as'`  [simpleIdentifier](#simpleIdentifier)  
 **;** 

<h4 id="topLevelObject">topLevelObject</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[kotlinFile](#kotlinFile)<span style="color:gray">)</span>  
 **:**  [declaration](#declaration) [semis](#semis)**?**   
 **;** 

<h4 id="typeAlias">typeAlias</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[declaration](#declaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**   `'typealias'`  [simpleIdentifier](#simpleIdentifier) [typeParameters](#typeParameters)**?**   `'='`  [type](#type)  
 **;** 

<h4 id="declaration">declaration</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[topLevelObject](#topLevelObject)<span style="color:gray">, </span>[classMemberDeclaration](#classMemberDeclaration)<span style="color:gray">, </span>[statement](#statement)<span style="color:gray">)</span>  
 **:**  [classDeclaration](#classDeclaration)  
 **|**  [objectDeclaration](#objectDeclaration)  
 **|**  [functionDeclaration](#functionDeclaration)  
 **|**  [propertyDeclaration](#propertyDeclaration)  
 **|**  [typeAlias](#typeAlias)  
 **;** 

### Classes

See [Classes and Inheritance](classes.html)


<h4 id="classDeclaration">classDeclaration</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[declaration](#declaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**  **(**  `'class'`  **|**   `'interface'` **)**   
   [simpleIdentifier](#simpleIdentifier) [typeParameters](#typeParameters)**?**   
   [primaryConstructor](#primaryConstructor)**?**   
   **(**  `':'`  [delegationSpecifiers](#delegationSpecifiers)**)** **?**   
   [typeConstraints](#typeConstraints)**?**   
   **(** [classBody](#classBody) **|**  [enumClassBody](#enumClassBody)**)** **?**   
 **;** 

<h4 id="primaryConstructor">primaryConstructor</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classDeclaration](#classDeclaration)<span style="color:gray">)</span>  
 **:**  **(** [modifiers](#modifiers)**?**   `'constructor'` **)** **?**  [classParameters](#classParameters)  
 **;** 

<h4 id="classBody">classBody</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classDeclaration](#classDeclaration)<span style="color:gray">, </span>[companionObject](#companionObject)<span style="color:gray">, </span>[objectDeclaration](#objectDeclaration)<span style="color:gray">, </span>[enumEntry](#enumEntry)<span style="color:gray">, </span>[objectLiteral](#objectLiteral)<span style="color:gray">)</span>  
 **:**   `'{'`  [classMemberDeclarations](#classMemberDeclarations)  `'}'`   
 **;** 

<h4 id="classParameters">classParameters</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryConstructor](#primaryConstructor)<span style="color:gray">)</span>  
 **:**   `'('`  **(** [classParameter](#classParameter) **(**  `','`  [classParameter](#classParameter)**)** **\*** **)** **?**   `')'`   
 **;** 

<h4 id="classParameter">classParameter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classParameters](#classParameters)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**  **(**  `'val'`  **|**   `'var'` **)** **?**  [simpleIdentifier](#simpleIdentifier)  `':'`  [type](#type) **(**  `'='`  [expression](#expression)**)** **?**   
 **;** 

<h4 id="delegationSpecifiers">delegationSpecifiers</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classDeclaration](#classDeclaration)<span style="color:gray">, </span>[companionObject](#companionObject)<span style="color:gray">, </span>[objectDeclaration](#objectDeclaration)<span style="color:gray">, </span>[objectLiteral](#objectLiteral)<span style="color:gray">)</span>  
 **:**  [annotatedDelegationSpecifier](#annotatedDelegationSpecifier) **(**  `','`  [annotatedDelegationSpecifier](#annotatedDelegationSpecifier)**)** **\***   
 **;** 

<h4 id="delegationSpecifier">delegationSpecifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotatedDelegationSpecifier](#annotatedDelegationSpecifier)<span style="color:gray">)</span>  
 **:**  [constructorInvocation](#constructorInvocation)  
 **|**  [explicitDelegation](#explicitDelegation)  
 **|**  [userType](#userType)  
 **|**  [functionType](#functionType)  
 **;** 

<h4 id="constructorInvocation">constructorInvocation</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[delegationSpecifier](#delegationSpecifier)<span style="color:gray">, </span>[unescapedAnnotation](#unescapedAnnotation)<span style="color:gray">)</span>  
 **:**  [userType](#userType) [valueArguments](#valueArguments)  
 **;** 

<h4 id="annotatedDelegationSpecifier">annotatedDelegationSpecifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[delegationSpecifiers](#delegationSpecifiers)<span style="color:gray">)</span>  
 **:**  [annotation](#annotation)**\***  [delegationSpecifier](#delegationSpecifier)  
 **;** 

<h4 id="explicitDelegation">explicitDelegation</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[delegationSpecifier](#delegationSpecifier)<span style="color:gray">)</span>  
 **:**  **(** [userType](#userType) **|**  [functionType](#functionType)**)**   `'by'`  [expression](#expression)  
 **;** 

See [Generic classes](generics.html)


<h4 id="typeParameters">typeParameters</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeAlias](#typeAlias)<span style="color:gray">, </span>[classDeclaration](#classDeclaration)<span style="color:gray">, </span>[functionDeclaration](#functionDeclaration)<span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">)</span>  
 **:**   `'<'`  [typeParameter](#typeParameter) **(**  `','`  [typeParameter](#typeParameter)**)** **\***   `'>'`   
 **;** 

<h4 id="typeParameter">typeParameter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeParameters](#typeParameters)<span style="color:gray">)</span>  
 **:**  [typeParameterModifiers](#typeParameterModifiers)**?**  [simpleIdentifier](#simpleIdentifier) **(**  `':'`  [type](#type)**)** **?**   
 **;** 

See [Generic constraints](generics.html#generic-constraints)


<h4 id="typeConstraints">typeConstraints</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classDeclaration](#classDeclaration)<span style="color:gray">, </span>[functionDeclaration](#functionDeclaration)<span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">, </span>[anonymousFunction](#anonymousFunction)<span style="color:gray">)</span>  
 **:**   `'where'`  [typeConstraint](#typeConstraint) **(**  `','`  [typeConstraint](#typeConstraint)**)** **\***   
 **;** 

<h4 id="typeConstraint">typeConstraint</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeConstraints](#typeConstraints)<span style="color:gray">)</span>  
 **:**  [annotation](#annotation)**\***  [simpleIdentifier](#simpleIdentifier)  `':'`  [type](#type)  
 **;** 

### Class members


<h4 id="classMemberDeclarations">classMemberDeclarations</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classBody](#classBody)<span style="color:gray">, </span>[enumClassBody](#enumClassBody)<span style="color:gray">)</span>  
 **:**  **(** [classMemberDeclaration](#classMemberDeclaration) [semis](#semis)**?** **)** **\***   
 **;** 

<h4 id="classMemberDeclaration">classMemberDeclaration</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classMemberDeclarations](#classMemberDeclarations)<span style="color:gray">)</span>  
 **:**  [declaration](#declaration)  
 **|**  [companionObject](#companionObject)  
 **|**  [anonymousInitializer](#anonymousInitializer)  
 **|**  [secondaryConstructor](#secondaryConstructor)  
 **;** 

<h4 id="anonymousInitializer">anonymousInitializer</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classMemberDeclaration](#classMemberDeclaration)<span style="color:gray">)</span>  
 **:**   `'init'`  [block](#block)  
 **;** 

<h4 id="companionObject">companionObject</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classMemberDeclaration](#classMemberDeclaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**   `'companion'`   `'object'`  [simpleIdentifier](#simpleIdentifier)**?**   
   **(**  `':'`  [delegationSpecifiers](#delegationSpecifiers)**)** **?**   
   [classBody](#classBody)**?**   
 **;** 

<h4 id="functionValueParameters">functionValueParameters</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[functionDeclaration](#functionDeclaration)<span style="color:gray">, </span>[secondaryConstructor](#secondaryConstructor)<span style="color:gray">, </span>[anonymousFunction](#anonymousFunction)<span style="color:gray">)</span>  
 **:**   `'('`  **(** [functionValueParameter](#functionValueParameter) **(**  `','`  [functionValueParameter](#functionValueParameter)**)** **\*** **)** **?**   `')'`   
 **;** 

<h4 id="functionValueParameter">functionValueParameter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[functionValueParameters](#functionValueParameters)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**  [parameter](#parameter) **(**  `'='`  [expression](#expression)**)** **?**   
 **;** 

<h4 id="functionDeclaration">functionDeclaration</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[declaration](#declaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**   `'fun'`  [typeParameters](#typeParameters)**?**   
   **(** [receiverType](#receiverType)  `'.'` **)** **?**   
   [simpleIdentifier](#simpleIdentifier) [functionValueParameters](#functionValueParameters)  
   **(**  `':'`  [type](#type)**)** **?**  [typeConstraints](#typeConstraints)**?**   
   [functionBody](#functionBody)**?**   
 **;** 

<h4 id="functionBody">functionBody</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[functionDeclaration](#functionDeclaration)<span style="color:gray">, </span>[getter](#getter)<span style="color:gray">, </span>[setter](#setter)<span style="color:gray">, </span>[anonymousFunction](#anonymousFunction)<span style="color:gray">)</span>  
 **:**  [block](#block)  
 **|**   `'='`  [expression](#expression)  
 **;** 

<h4 id="variableDeclaration">variableDeclaration</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[multiVariableDeclaration](#multiVariableDeclaration)<span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">, </span>[forStatement](#forStatement)<span style="color:gray">, </span>[lambdaParameter](#lambdaParameter)<span style="color:gray">, </span>[whenSubject](#whenSubject)<span style="color:gray">)</span>  
 **:**  [annotation](#annotation)**\***  [simpleIdentifier](#simpleIdentifier) **(**  `':'`  [type](#type)**)** **?**   
 **;** 

<h4 id="multiVariableDeclaration">multiVariableDeclaration</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">, </span>[forStatement](#forStatement)<span style="color:gray">, </span>[lambdaParameter](#lambdaParameter)<span style="color:gray">)</span>  
 **:**   `'('`  [variableDeclaration](#variableDeclaration) **(**  `','`  [variableDeclaration](#variableDeclaration)**)** **\***   `')'`   
 **;** 

See [Properties and Fields](properties.html)


<h4 id="propertyDeclaration">propertyDeclaration</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[declaration](#declaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**  **(**  `'val'`  **|**   `'var'` **)**  [typeParameters](#typeParameters)**?**   
   **(** [receiverType](#receiverType)  `'.'` **)** **?**   
   **(** [multiVariableDeclaration](#multiVariableDeclaration) **|**  [variableDeclaration](#variableDeclaration)**)**   
   [typeConstraints](#typeConstraints)**?**   
   **(** **(**  `'='`  [expression](#expression)**)**  **|**  [propertyDelegate](#propertyDelegate)**)** **?**   `';'` **?**   
   **(** **(** [getter](#getter)**?**  **(** [semi](#semi)**?**  [setter](#setter)**)** **?** **)**  **|**  **(** [setter](#setter)**?**  **(** [semi](#semi)**?**  [getter](#getter)**)** **?** **)** **)**   
 **;** 

<h4 id="propertyDelegate">propertyDelegate</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">)</span>  
 **:**   `'by'`  [expression](#expression)  
 **;** 

<h4 id="getter">getter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**   `'get'`   
 **|**  [modifiers](#modifiers)**?**   `'get'`   `'('`   `')'`   
   **(**  `':'`  [type](#type)**)** **?**   
   [functionBody](#functionBody)  
 **;** 

<h4 id="setter">setter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**   `'set'`   
 **|**  [modifiers](#modifiers)**?**   `'set'`   `'('`  **(** [annotation](#annotation) **|**  [parameterModifier](#parameterModifier)**)** **\***  [setterParameter](#setterParameter)  `')'`   
   **(**  `':'`  [type](#type)**)** **?**   
   [functionBody](#functionBody)  
 **;** 

<h4 id="setterParameter">setterParameter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[setter](#setter)<span style="color:gray">)</span>  
 **:**  [simpleIdentifier](#simpleIdentifier) **(**  `':'`  [type](#type)**)** **?**   
 **;** 

<h4 id="parameter">parameter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[functionValueParameter](#functionValueParameter)<span style="color:gray">, </span>[functionTypeParameters](#functionTypeParameters)<span style="color:gray">)</span>  
 **:**  [simpleIdentifier](#simpleIdentifier)  `':'`  [type](#type)  
 **;** 

See [Object expressions and Declarations](object-declarations.html)


<h4 id="objectDeclaration">objectDeclaration</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[declaration](#declaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**   `'object'`  [simpleIdentifier](#simpleIdentifier) **(**  `':'`  [delegationSpecifiers](#delegationSpecifiers)**)** **?**  [classBody](#classBody)**?**   
 **;** 

<h4 id="secondaryConstructor">secondaryConstructor</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classMemberDeclaration](#classMemberDeclaration)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**   `'constructor'`  [functionValueParameters](#functionValueParameters)  
   **(**  `':'`  [constructorDelegationCall](#constructorDelegationCall)**)** **?**  [block](#block)**?**   
 **;** 

<h4 id="constructorDelegationCall">constructorDelegationCall</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[secondaryConstructor](#secondaryConstructor)<span style="color:gray">)</span>  
 **:**   `'this'`  [valueArguments](#valueArguments)  
 **|**   `'super'`  [valueArguments](#valueArguments)  
 **;** 

### Enum classes

See [Enum classes](enum-classes.html)


<h4 id="enumClassBody">enumClassBody</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classDeclaration](#classDeclaration)<span style="color:gray">)</span>  
 **:**   `'{'`  [enumEntries](#enumEntries)**?**  **(**  `';'`  [classMemberDeclarations](#classMemberDeclarations)**)** **?**   `'}'`   
 **;** 

<h4 id="enumEntries">enumEntries</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[enumClassBody](#enumClassBody)<span style="color:gray">)</span>  
 **:**  [enumEntry](#enumEntry) **(**  `','`  [enumEntry](#enumEntry)**)** **\***   `','` **?**   
 **;** 

<h4 id="enumEntry">enumEntry</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[enumEntries](#enumEntries)<span style="color:gray">)</span>  
 **:**  [modifiers](#modifiers)**?**  [simpleIdentifier](#simpleIdentifier) [valueArguments](#valueArguments)**?**  [classBody](#classBody)**?**   
 **;** 

### Types

See [Types](basic-types.html)


<h4 id="type">type</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeAlias](#typeAlias)<span style="color:gray">, </span>[classParameter](#classParameter)<span style="color:gray">, </span>[typeParameter](#typeParameter)<span style="color:gray">, </span>[typeConstraint](#typeConstraint)<span style="color:gray">, </span>[functionDeclaration](#functionDeclaration)<span style="color:gray">, </span>[variableDeclaration](#variableDeclaration)<span style="color:gray">, </span>[getter](#getter)<span style="color:gray">, </span>[setter](#setter)<span style="color:gray">, </span>[setterParameter](#setterParameter)<span style="color:gray">, </span>[parameter](#parameter)<span style="color:gray">, </span>[typeProjection](#typeProjection)<span style="color:gray">, </span>[functionType](#functionType)<span style="color:gray">, </span>[functionTypeParameters](#functionTypeParameters)<span style="color:gray">, </span>[parenthesizedType](#parenthesizedType)<span style="color:gray">, </span>[infixOperation](#infixOperation)<span style="color:gray">, </span>[asExpression](#asExpression)<span style="color:gray">, </span>[lambdaParameter](#lambdaParameter)<span style="color:gray">, </span>[anonymousFunction](#anonymousFunction)<span style="color:gray">, </span>[superExpression](#superExpression)<span style="color:gray">, </span>[typeTest](#typeTest)<span style="color:gray">, </span>[catchBlock](#catchBlock)<span style="color:gray">)</span>  
 **:**  [typeModifiers](#typeModifiers)**?**  **(** [parenthesizedType](#parenthesizedType) **|**  [nullableType](#nullableType) **|**  [typeReference](#typeReference) **|**  [functionType](#functionType)**)**   
 **;** 

<h4 id="typeReference">typeReference</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[type](#type)<span style="color:gray">, </span>[nullableType](#nullableType)<span style="color:gray">, </span>[receiverType](#receiverType)<span style="color:gray">)</span>  
 **:**  [userType](#userType)  
 **|**   `'dynamic'`   
 **;** 

<h4 id="nullableType">nullableType</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[type](#type)<span style="color:gray">, </span>[receiverType](#receiverType)<span style="color:gray">)</span>  
 **:**  **(** [typeReference](#typeReference) **|**  [parenthesizedType](#parenthesizedType)**)**  [quest](#quest)**+**   
 **;** 

<h4 id="quest">quest</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[nullableType](#nullableType)<span style="color:gray">)</span>  
 **:**   `'?'`   
 **|**  [QUEST_WS](#QUEST_WS)  
 **;** 

<h4 id="userType">userType</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[delegationSpecifier](#delegationSpecifier)<span style="color:gray">, </span>[constructorInvocation](#constructorInvocation)<span style="color:gray">, </span>[explicitDelegation](#explicitDelegation)<span style="color:gray">, </span>[typeReference](#typeReference)<span style="color:gray">, </span>[parenthesizedUserType](#parenthesizedUserType)<span style="color:gray">, </span>[unescapedAnnotation](#unescapedAnnotation)<span style="color:gray">)</span>  
 **:**  [simpleUserType](#simpleUserType) **(**  `'.'`  [simpleUserType](#simpleUserType)**)** **\***   
 **;** 

<h4 id="simpleUserType">simpleUserType</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[userType](#userType)<span style="color:gray">)</span>  
 **:**  [simpleIdentifier](#simpleIdentifier) [typeArguments](#typeArguments)**?**   
 **;** 

<h4 id="typeProjection">typeProjection</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeArguments](#typeArguments)<span style="color:gray">)</span>  
 **:**  [typeProjectionModifiers](#typeProjectionModifiers)**?**  [type](#type)  
 **|**   `'*'`   
 **;** 

<h4 id="typeProjectionModifiers">typeProjectionModifiers</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeProjection](#typeProjection)<span style="color:gray">)</span>  
 **:**  [typeProjectionModifier](#typeProjectionModifier)**+**   
 **;** 

<h4 id="typeProjectionModifier">typeProjectionModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeProjectionModifiers](#typeProjectionModifiers)<span style="color:gray">)</span>  
 **:**  [varianceModifier](#varianceModifier)  
 **|**  [annotation](#annotation)  
 **;** 

<h4 id="functionType">functionType</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[delegationSpecifier](#delegationSpecifier)<span style="color:gray">, </span>[explicitDelegation](#explicitDelegation)<span style="color:gray">, </span>[type](#type)<span style="color:gray">)</span>  
 **:**  **(** [receiverType](#receiverType)  `'.'` **)** **?**  [functionTypeParameters](#functionTypeParameters)  `'->'`  [type](#type)  
 **;** 

<h4 id="functionTypeParameters">functionTypeParameters</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[functionType](#functionType)<span style="color:gray">)</span>  
 **:**   `'('`  **(** [parameter](#parameter) **|**  [type](#type)**)** **?**  **(**  `','`  **(** [parameter](#parameter) **|**  [type](#type)**)** **)** **\***   `')'`   
 **;** 

<h4 id="parenthesizedType">parenthesizedType</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[type](#type)<span style="color:gray">, </span>[nullableType](#nullableType)<span style="color:gray">, </span>[receiverType](#receiverType)<span style="color:gray">)</span>  
 **:**   `'('`  [type](#type)  `')'`   
 **;** 

<h4 id="receiverType">receiverType</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[functionDeclaration](#functionDeclaration)<span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">, </span>[functionType](#functionType)<span style="color:gray">, </span>[callableReference](#callableReference)<span style="color:gray">)</span>  
 **:**  [typeModifiers](#typeModifiers)**?**  **(** [parenthesizedType](#parenthesizedType) **|**  [nullableType](#nullableType) **|**  [typeReference](#typeReference)**)**   
 **;** 

<h4 id="parenthesizedUserType">parenthesizedUserType</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[parenthesizedUserType](#parenthesizedUserType)<span style="color:gray">)</span>  
 **:**   `'('`  [userType](#userType)  `')'`   
 **|**   `'('`  [parenthesizedUserType](#parenthesizedUserType)  `')'`   
 **;** 

### Statements


<h4 id="statements">statements</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[block](#block)<span style="color:gray">, </span>[lambdaLiteral](#lambdaLiteral)<span style="color:gray">)</span>  
 **:**  **(** [statement](#statement) **(** [semis](#semis) [statement](#statement)**)** **\***  [semis](#semis)**?** **)** **?**   
 **;** 

<h4 id="statement">statement</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[script](#script)<span style="color:gray">, </span>[statements](#statements)<span style="color:gray">, </span>[controlStructureBody](#controlStructureBody)<span style="color:gray">)</span>  
 **:**  **(** [label](#label) **|**  [annotation](#annotation)**)** **\***  **(** [declaration](#declaration) **|**  [assignment](#assignment) **|**  [loopStatement](#loopStatement) **|**  [expression](#expression)**)**   
 **;** 

See [Returns and jumps](returns.html)


<h4 id="label">label</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[statement](#statement)<span style="color:gray">, </span>[unaryPrefix](#unaryPrefix)<span style="color:gray">, </span>[annotatedLambda](#annotatedLambda)<span style="color:gray">)</span>  
 **:**  [IdentifierAt](#IdentifierAt)  
 **;** 

<h4 id="controlStructureBody">controlStructureBody</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[forStatement](#forStatement)<span style="color:gray">, </span>[whileStatement](#whileStatement)<span style="color:gray">, </span>[doWhileStatement](#doWhileStatement)<span style="color:gray">, </span>[ifExpression](#ifExpression)<span style="color:gray">, </span>[whenEntry](#whenEntry)<span style="color:gray">)</span>  
 **:**  [block](#block)  
 **|**  [statement](#statement)  
 **;** 

<h4 id="block">block</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[anonymousInitializer](#anonymousInitializer)<span style="color:gray">, </span>[functionBody](#functionBody)<span style="color:gray">, </span>[secondaryConstructor](#secondaryConstructor)<span style="color:gray">, </span>[controlStructureBody](#controlStructureBody)<span style="color:gray">, </span>[tryExpression](#tryExpression)<span style="color:gray">, </span>[catchBlock](#catchBlock)<span style="color:gray">, </span>[finallyBlock](#finallyBlock)<span style="color:gray">)</span>  
 **:**   `'{'`  [statements](#statements)  `'}'`   
 **;** 

<h4 id="loopStatement">loopStatement</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[statement](#statement)<span style="color:gray">)</span>  
 **:**  [forStatement](#forStatement)  
 **|**  [whileStatement](#whileStatement)  
 **|**  [doWhileStatement](#doWhileStatement)  
 **;** 

<h4 id="forStatement">forStatement</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[loopStatement](#loopStatement)<span style="color:gray">)</span>  
 **:**   `'for'`   
    `'('`  [annotation](#annotation)**\***  **(** [variableDeclaration](#variableDeclaration) **|**  [multiVariableDeclaration](#multiVariableDeclaration)**)**   `'in'`  [expression](#expression)  `')'`   
   [controlStructureBody](#controlStructureBody)**?**   
 **;** 

<h4 id="whileStatement">whileStatement</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[loopStatement](#loopStatement)<span style="color:gray">)</span>  
 **:**   `'while'`   `'('`  [expression](#expression)  `')'`  [controlStructureBody](#controlStructureBody)  
 **|**   `'while'`   `'('`  [expression](#expression)  `')'`   `';'`   
 **;** 

<h4 id="doWhileStatement">doWhileStatement</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[loopStatement](#loopStatement)<span style="color:gray">)</span>  
 **:**   `'do'`  [controlStructureBody](#controlStructureBody)**?**   `'while'`   `'('`  [expression](#expression)  `')'`   
 **;** 

<h4 id="assignment">assignment</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[statement](#statement)<span style="color:gray">)</span>  
 **:**  [directlyAssignableExpression](#directlyAssignableExpression)  `'='`  [expression](#expression)  
 **|**  [assignableExpression](#assignableExpression) [assignmentAndOperator](#assignmentAndOperator) [expression](#expression)  
 **;** 

<h4 id="semi">semi</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[script](#script)<span style="color:gray">, </span>[packageHeader](#packageHeader)<span style="color:gray">, </span>[importHeader](#importHeader)<span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">, </span>[whenEntry](#whenEntry)<span style="color:gray">)</span>  
 **:**   `EOF`   
 **;** 

<h4 id="semis">semis</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[topLevelObject](#topLevelObject)<span style="color:gray">, </span>[classMemberDeclarations](#classMemberDeclarations)<span style="color:gray">, </span>[statements](#statements)<span style="color:gray">)</span>  
 **:**   `EOF`   
 **;** 

### Expressions

| Precedence | Title | Symbols |
|------------|-------|---------|
| Highest    | Postfix | `++`, `--`, `.`, `?.`, `?` |
| | Prefix | `-`, `+`, `++`, `--`, `!`, [`label`](#label) |
| | Type RHS | `:`, `as`, `as?` |
| | Multiplicative | `*`, `/`, `%` |
| | Additive | `+`, `-` |
| | Range | `..` |
| | Infix function | [`simpleIdentifier`](#simpleIdentifier) |
| | Elvis | `?:` |
| | Named checks | `in`, `!in`, `is`, `!is` |
| | Comparison | `<`, `>`, `<=`, `>=` |
| | Equality | `==`, `!==` |
| | Conjunction | `&&` |
| | Disjunction | `||` |
| | Spread operator | `*` |
| Lowest | Assignment | `=`, `+=`, `-=`, `*=`, `/=`, `%=` |


<h4 id="expression">expression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[classParameter](#classParameter)<span style="color:gray">, </span>[explicitDelegation](#explicitDelegation)<span style="color:gray">, </span>[functionValueParameter](#functionValueParameter)<span style="color:gray">, </span>[functionBody](#functionBody)<span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">, </span>[propertyDelegate](#propertyDelegate)<span style="color:gray">, </span>[statement](#statement)<span style="color:gray">, </span>[forStatement](#forStatement)<span style="color:gray">, </span>[whileStatement](#whileStatement)<span style="color:gray">, </span>[doWhileStatement](#doWhileStatement)<span style="color:gray">, </span>[assignment](#assignment)<span style="color:gray">, </span>[indexingSuffix](#indexingSuffix)<span style="color:gray">, </span>[valueArgument](#valueArgument)<span style="color:gray">, </span>[parenthesizedExpression](#parenthesizedExpression)<span style="color:gray">, </span>[collectionLiteral](#collectionLiteral)<span style="color:gray">, </span>[lineStringExpression](#lineStringExpression)<span style="color:gray">, </span>[multiLineStringExpression](#multiLineStringExpression)<span style="color:gray">, </span>[ifExpression](#ifExpression)<span style="color:gray">, </span>[whenSubject](#whenSubject)<span style="color:gray">, </span>[whenCondition](#whenCondition)<span style="color:gray">, </span>[rangeTest](#rangeTest)<span style="color:gray">, </span>[jumpExpression](#jumpExpression)<span style="color:gray">)</span>  
 **:**  [disjunction](#disjunction)  
 **;** 

<h4 id="disjunction">disjunction</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[expression](#expression)<span style="color:gray">)</span>  
 **:**  [conjunction](#conjunction) **(**  `'||'`  [conjunction](#conjunction)**)** **\***   
 **;** 

<h4 id="conjunction">conjunction</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[disjunction](#disjunction)<span style="color:gray">)</span>  
 **:**  [equality](#equality) **(**  `'&&'`  [equality](#equality)**)** **\***   
 **;** 

<h4 id="equality">equality</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[conjunction](#conjunction)<span style="color:gray">)</span>  
 **:**  [comparison](#comparison) **(** [equalityOperator](#equalityOperator) [comparison](#comparison)**)** **\***   
 **;** 

<h4 id="comparison">comparison</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[equality](#equality)<span style="color:gray">)</span>  
 **:**  [infixOperation](#infixOperation) **(** [comparisonOperator](#comparisonOperator) [infixOperation](#infixOperation)**)** **?**   
 **;** 

<h4 id="infixOperation">infixOperation</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[comparison](#comparison)<span style="color:gray">)</span>  
 **:**  [elvisExpression](#elvisExpression) **(** **(** [inOperator](#inOperator) [elvisExpression](#elvisExpression)**)**  **|**  **(** [isOperator](#isOperator) [type](#type)**)** **)** **\***   
 **;** 

<h4 id="elvisExpression">elvisExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[infixOperation](#infixOperation)<span style="color:gray">)</span>  
 **:**  [infixFunctionCall](#infixFunctionCall) **(** [elvis](#elvis) [infixFunctionCall](#infixFunctionCall)**)** **\***   
 **;** 

<h4 id="elvis">elvis</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[elvisExpression](#elvisExpression)<span style="color:gray">)</span>  
 **:**   `'?'`   `':'`   
 **;** 

<h4 id="infixFunctionCall">infixFunctionCall</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[elvisExpression](#elvisExpression)<span style="color:gray">)</span>  
 **:**  [rangeExpression](#rangeExpression) **(** [simpleIdentifier](#simpleIdentifier) [rangeExpression](#rangeExpression)**)** **\***   
 **;** 

<h4 id="rangeExpression">rangeExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[infixFunctionCall](#infixFunctionCall)<span style="color:gray">)</span>  
 **:**  [additiveExpression](#additiveExpression) **(**  `'..'`  [additiveExpression](#additiveExpression)**)** **\***   
 **;** 

<h4 id="additiveExpression">additiveExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[rangeExpression](#rangeExpression)<span style="color:gray">)</span>  
 **:**  [multiplicativeExpression](#multiplicativeExpression) **(** [additiveOperator](#additiveOperator) [multiplicativeExpression](#multiplicativeExpression)**)** **\***   
 **;** 

<h4 id="multiplicativeExpression">multiplicativeExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[additiveExpression](#additiveExpression)<span style="color:gray">)</span>  
 **:**  [asExpression](#asExpression) **(** [multiplicativeOperator](#multiplicativeOperator) [asExpression](#asExpression)**)** **\***   
 **;** 

<h4 id="asExpression">asExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[multiplicativeExpression](#multiplicativeExpression)<span style="color:gray">)</span>  
 **:**  [prefixUnaryExpression](#prefixUnaryExpression) **(** [asOperator](#asOperator) [type](#type)**)** **?**   
 **;** 

<h4 id="prefixUnaryExpression">prefixUnaryExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[asExpression](#asExpression)<span style="color:gray">, </span>[assignableExpression](#assignableExpression)<span style="color:gray">)</span>  
 **:**  [unaryPrefix](#unaryPrefix)**\***  [postfixUnaryExpression](#postfixUnaryExpression)  
 **;** 

<h4 id="unaryPrefix">unaryPrefix</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[prefixUnaryExpression](#prefixUnaryExpression)<span style="color:gray">)</span>  
 **:**  [annotation](#annotation)  
 **|**  [label](#label)  
 **|**  [prefixUnaryOperator](#prefixUnaryOperator)  
 **;** 

<h4 id="postfixUnaryExpression">postfixUnaryExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[prefixUnaryExpression](#prefixUnaryExpression)<span style="color:gray">, </span>[directlyAssignableExpression](#directlyAssignableExpression)<span style="color:gray">)</span>  
 **:**  [primaryExpression](#primaryExpression)  
 **|**  [primaryExpression](#primaryExpression) [postfixUnarySuffix](#postfixUnarySuffix)**+**   
 **;** 

<h4 id="postfixUnarySuffix">postfixUnarySuffix</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[postfixUnaryExpression](#postfixUnaryExpression)<span style="color:gray">)</span>  
 **:**  [postfixUnaryOperator](#postfixUnaryOperator)  
 **|**  [typeArguments](#typeArguments)  
 **|**  [callSuffix](#callSuffix)  
 **|**  [indexingSuffix](#indexingSuffix)  
 **|**  [navigationSuffix](#navigationSuffix)  
 **;** 

<h4 id="directlyAssignableExpression">directlyAssignableExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[assignment](#assignment)<span style="color:gray">)</span>  
 **:**  [postfixUnaryExpression](#postfixUnaryExpression) [assignableSuffix](#assignableSuffix)  
 **|**  [simpleIdentifier](#simpleIdentifier)  
 **;** 

<h4 id="assignableExpression">assignableExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[assignment](#assignment)<span style="color:gray">)</span>  
 **:**  [prefixUnaryExpression](#prefixUnaryExpression)  
 **;** 

<h4 id="assignableSuffix">assignableSuffix</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[directlyAssignableExpression](#directlyAssignableExpression)<span style="color:gray">)</span>  
 **:**  [typeArguments](#typeArguments)  
 **|**  [indexingSuffix](#indexingSuffix)  
 **|**  [navigationSuffix](#navigationSuffix)  
 **;** 

<h4 id="indexingSuffix">indexingSuffix</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[postfixUnarySuffix](#postfixUnarySuffix)<span style="color:gray">, </span>[assignableSuffix](#assignableSuffix)<span style="color:gray">)</span>  
 **:**   `'['`  [expression](#expression) **(**  `','`  [expression](#expression)**)** **\***   `']'`   
 **;** 

<h4 id="navigationSuffix">navigationSuffix</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[postfixUnarySuffix](#postfixUnarySuffix)<span style="color:gray">, </span>[assignableSuffix](#assignableSuffix)<span style="color:gray">)</span>  
 **:**  [memberAccessOperator](#memberAccessOperator) **(** [simpleIdentifier](#simpleIdentifier) **|**  [parenthesizedExpression](#parenthesizedExpression) **|**   `'class'` **)**   
 **;** 

<h4 id="callSuffix">callSuffix</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[postfixUnarySuffix](#postfixUnarySuffix)<span style="color:gray">)</span>  
 **:**  [typeArguments](#typeArguments)**?**  [valueArguments](#valueArguments)**?**  [annotatedLambda](#annotatedLambda)  
 **|**  [typeArguments](#typeArguments)**?**  [valueArguments](#valueArguments)  
 **;** 

<h4 id="annotatedLambda">annotatedLambda</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[callSuffix](#callSuffix)<span style="color:gray">)</span>  
 **:**  [annotation](#annotation)**\***  [label](#label)**?**  [lambdaLiteral](#lambdaLiteral)  
 **;** 

<h4 id="typeArguments">typeArguments</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[simpleUserType](#simpleUserType)<span style="color:gray">, </span>[postfixUnarySuffix](#postfixUnarySuffix)<span style="color:gray">, </span>[assignableSuffix](#assignableSuffix)<span style="color:gray">, </span>[callSuffix](#callSuffix)<span style="color:gray">)</span>  
 **:**   `'<'`  [typeProjection](#typeProjection) **(**  `','`  [typeProjection](#typeProjection)**)** **\***   `'>'`   
 **;** 

<h4 id="valueArguments">valueArguments</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[constructorInvocation](#constructorInvocation)<span style="color:gray">, </span>[constructorDelegationCall](#constructorDelegationCall)<span style="color:gray">, </span>[enumEntry](#enumEntry)<span style="color:gray">, </span>[callSuffix](#callSuffix)<span style="color:gray">)</span>  
 **:**   `'('`   `')'`   
 **|**   `'('`  [valueArgument](#valueArgument) **(**  `','`  [valueArgument](#valueArgument)**)** **\***   `')'`   
 **;** 

<h4 id="valueArgument">valueArgument</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[valueArguments](#valueArguments)<span style="color:gray">)</span>  
 **:**  [annotation](#annotation)**?**  **(** [simpleIdentifier](#simpleIdentifier)  `'='` **)** **?**   `'*'` **?**  [expression](#expression)  
 **;** 

<h4 id="primaryExpression">primaryExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[postfixUnaryExpression](#postfixUnaryExpression)<span style="color:gray">)</span>  
 **:**  [parenthesizedExpression](#parenthesizedExpression)  
 **|**  [simpleIdentifier](#simpleIdentifier)  
 **|**  [literalConstant](#literalConstant)  
 **|**  [stringLiteral](#stringLiteral)  
 **|**  [callableReference](#callableReference)  
 **|**  [functionLiteral](#functionLiteral)  
 **|**  [objectLiteral](#objectLiteral)  
 **|**  [collectionLiteral](#collectionLiteral)  
 **|**  [thisExpression](#thisExpression)  
 **|**  [superExpression](#superExpression)  
 **|**  [ifExpression](#ifExpression)  
 **|**  [whenExpression](#whenExpression)  
 **|**  [tryExpression](#tryExpression)  
 **|**  [jumpExpression](#jumpExpression)  
 **;** 

<h4 id="parenthesizedExpression">parenthesizedExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[navigationSuffix](#navigationSuffix)<span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'('`  [expression](#expression)  `')'`   
 **;** 

<h4 id="collectionLiteral">collectionLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'['`  [expression](#expression) **(**  `','`  [expression](#expression)**)** **\***   `']'`   
 **|**   `'['`   `']'`   
 **;** 

<h4 id="literalConstant">literalConstant</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**  [BooleanLiteral](#BooleanLiteral)  
 **|**  [IntegerLiteral](#IntegerLiteral)  
 **|**  [HexLiteral](#HexLiteral)  
 **|**  [BinLiteral](#BinLiteral)  
 **|**  [CharacterLiteral](#CharacterLiteral)  
 **|**  [RealLiteral](#RealLiteral)  
 **|**   `'null'`   
 **|**  [LongLiteral](#LongLiteral)  
 **|**  [UnsignedLiteral](#UnsignedLiteral)  
 **;** 

<h4 id="stringLiteral">stringLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**  [lineStringLiteral](#lineStringLiteral)  
 **|**  [multiLineStringLiteral](#multiLineStringLiteral)  
 **;** 

<h4 id="lineStringLiteral">lineStringLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[stringLiteral](#stringLiteral)<span style="color:gray">)</span>  
 **:**   `'"'`  **(** [lineStringContent](#lineStringContent) **|**  [lineStringExpression](#lineStringExpression)**)** **\***   `'"'`   
 **;** 

<h4 id="multiLineStringLiteral">multiLineStringLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[stringLiteral](#stringLiteral)<span style="color:gray">)</span>  
 **:**   `'"""'`  **(** [multiLineStringContent](#multiLineStringContent) **|**  [multiLineStringExpression](#multiLineStringExpression) **|**   `'"'` **)** **\***   
   [TRIPLE_QUOTE_CLOSE](#TRIPLE_QUOTE_CLOSE)  
 **;** 

<h4 id="lineStringContent">lineStringContent</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[lineStringLiteral](#lineStringLiteral)<span style="color:gray">)</span>  
 **:**  [LineStrText](#LineStrText)  
 **|**  [LineStrEscapedChar](#LineStrEscapedChar)  
 **|**  [LineStrRef](#LineStrRef)  
 **;** 

<h4 id="lineStringExpression">lineStringExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[lineStringLiteral](#lineStringLiteral)<span style="color:gray">)</span>  
 **:**   `'${'`  [expression](#expression)  `'}'`   
 **;** 

<h4 id="multiLineStringContent">multiLineStringContent</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[multiLineStringLiteral](#multiLineStringLiteral)<span style="color:gray">)</span>  
 **:**  [MultiLineStrText](#MultiLineStrText)  
 **|**   `'"'`   
 **|**  [MultiLineStrRef](#MultiLineStrRef)  
 **;** 

<h4 id="multiLineStringExpression">multiLineStringExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[multiLineStringLiteral](#multiLineStringLiteral)<span style="color:gray">)</span>  
 **:**   `'${'`  [expression](#expression)  `'}'`   
 **;** 

<h4 id="lambdaLiteral">lambdaLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotatedLambda](#annotatedLambda)<span style="color:gray">, </span>[functionLiteral](#functionLiteral)<span style="color:gray">)</span>  
 **:**   `'{'`  [statements](#statements)  `'}'`   
 **|**   `'{'`  [lambdaParameters](#lambdaParameters)**?**   `'->'`  [statements](#statements)  `'}'`   
 **;** 

<h4 id="lambdaParameters">lambdaParameters</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[lambdaLiteral](#lambdaLiteral)<span style="color:gray">)</span>  
 **:**  [lambdaParameter](#lambdaParameter) **(**  `','`  [lambdaParameter](#lambdaParameter)**)** **\***   
 **;** 

<h4 id="lambdaParameter">lambdaParameter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[lambdaParameters](#lambdaParameters)<span style="color:gray">)</span>  
 **:**  [variableDeclaration](#variableDeclaration)  
 **|**  [multiVariableDeclaration](#multiVariableDeclaration) **(**  `':'`  [type](#type)**)** **?**   
 **;** 

<h4 id="anonymousFunction">anonymousFunction</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[functionLiteral](#functionLiteral)<span style="color:gray">)</span>  
 **:**   `'fun'`  **(** [type](#type)  `'.'` **)** **?**  [functionValueParameters](#functionValueParameters)  
   **(**  `':'`  [type](#type)**)** **?**  [typeConstraints](#typeConstraints)**?**   
   [functionBody](#functionBody)**?**   
 **;** 

<h4 id="functionLiteral">functionLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**  [lambdaLiteral](#lambdaLiteral)  
 **|**  [anonymousFunction](#anonymousFunction)  
 **;** 

<h4 id="objectLiteral">objectLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'object'`   `':'`  [delegationSpecifiers](#delegationSpecifiers) [classBody](#classBody)  
 **|**   `'object'`  [classBody](#classBody)  
 **;** 

<h4 id="thisExpression">thisExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'this'`   
 **|**  [THIS_AT](#THIS_AT)  
 **;** 

<h4 id="superExpression">superExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'super'`  **(**  `'<'`  [type](#type)  `'>'` **)** **?**  **(**  `'@'`  [simpleIdentifier](#simpleIdentifier)**)** **?**   
 **|**  [SUPER_AT](#SUPER_AT)  
 **;** 

<h4 id="ifExpression">ifExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'if'`   `'('`  [expression](#expression)  `')'`   
   **(** [controlStructureBody](#controlStructureBody) **|**   `';'` **)**   
 **|**   `'if'`   `'('`  [expression](#expression)  `')'`   
   [controlStructureBody](#controlStructureBody)**?**   `';'` **?**   `'else'`  **(** [controlStructureBody](#controlStructureBody) **|**   `';'` **)**   
 **;** 

<h4 id="whenSubject">whenSubject</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[whenExpression](#whenExpression)<span style="color:gray">)</span>  
 **:**   `'('`  **(** [annotation](#annotation)**\***   `'val'`  [variableDeclaration](#variableDeclaration)  `'='` **)** **?**  [expression](#expression)  `')'`   
 **;** 

<h4 id="whenExpression">whenExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'when'`  [whenSubject](#whenSubject)**?**   `'{'`  [whenEntry](#whenEntry)**\***   `'}'`   
 **;** 

<h4 id="whenEntry">whenEntry</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[whenExpression](#whenExpression)<span style="color:gray">)</span>  
 **:**  [whenCondition](#whenCondition) **(**  `','`  [whenCondition](#whenCondition)**)** **\***   `'->'`  [controlStructureBody](#controlStructureBody) [semi](#semi)**?**   
 **|**   `'else'`   `'->'`  [controlStructureBody](#controlStructureBody) [semi](#semi)**?**   
 **;** 

<h4 id="whenCondition">whenCondition</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[whenEntry](#whenEntry)<span style="color:gray">)</span>  
 **:**  [expression](#expression)  
 **|**  [rangeTest](#rangeTest)  
 **|**  [typeTest](#typeTest)  
 **;** 

<h4 id="rangeTest">rangeTest</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[whenCondition](#whenCondition)<span style="color:gray">)</span>  
 **:**  [inOperator](#inOperator) [expression](#expression)  
 **;** 

<h4 id="typeTest">typeTest</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[whenCondition](#whenCondition)<span style="color:gray">)</span>  
 **:**  [isOperator](#isOperator) [type](#type)  
 **;** 

<h4 id="tryExpression">tryExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'try'`  [block](#block) **(** **(** [catchBlock](#catchBlock)**+**  [finallyBlock](#finallyBlock)**?** **)**  **|**  [finallyBlock](#finallyBlock)**)**   
 **;** 

<h4 id="catchBlock">catchBlock</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[tryExpression](#tryExpression)<span style="color:gray">)</span>  
 **:**   `'catch'`   `'('`  [annotation](#annotation)**\***  [simpleIdentifier](#simpleIdentifier)  `':'`  [type](#type)  `')'`  [block](#block)  
 **;** 

<h4 id="finallyBlock">finallyBlock</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[tryExpression](#tryExpression)<span style="color:gray">)</span>  
 **:**   `'finally'`  [block](#block)  
 **;** 

<h4 id="jumpExpression">jumpExpression</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**   `'throw'`  [expression](#expression)  
 **|**  **(**  `'return'`  **|**  [RETURN_AT](#RETURN_AT)**)**  [expression](#expression)**?**   
 **|**   `'continue'`   
 **|**  [CONTINUE_AT](#CONTINUE_AT)  
 **|**   `'break'`   
 **|**  [BREAK_AT](#BREAK_AT)  
 **;** 

<h4 id="callableReference">callableReference</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">)</span>  
 **:**  **(** [receiverType](#receiverType)**?**   `'::'`  **(** [simpleIdentifier](#simpleIdentifier) **|**   `'class'` **)** **)**   
 **;** 

<h4 id="assignmentAndOperator">assignmentAndOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[assignment](#assignment)<span style="color:gray">)</span>  
 **:**   `'+='`   
 **|**   `'-='`   
 **|**   `'*='`   
 **|**   `'/='`   
 **|**   `'%='`   
 **;** 

<h4 id="equalityOperator">equalityOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[equality](#equality)<span style="color:gray">)</span>  
 **:**   `'!='`   
 **|**   `'!=='`   
 **|**   `'=='`   
 **|**   `'==='`   
 **;** 

<h4 id="comparisonOperator">comparisonOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[comparison](#comparison)<span style="color:gray">)</span>  
 **:**   `'<'`   
 **|**   `'>'`   
 **|**   `'<='`   
 **|**   `'>='`   
 **;** 

<h4 id="inOperator">inOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[infixOperation](#infixOperation)<span style="color:gray">, </span>[rangeTest](#rangeTest)<span style="color:gray">)</span>  
 **:**   `'in'`   
 **|**  [NOT_IN](#NOT_IN)  
 **;** 

<h4 id="isOperator">isOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[infixOperation](#infixOperation)<span style="color:gray">, </span>[typeTest](#typeTest)<span style="color:gray">)</span>  
 **:**   `'is'`   
 **|**  [NOT_IS](#NOT_IS)  
 **;** 

<h4 id="additiveOperator">additiveOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[additiveExpression](#additiveExpression)<span style="color:gray">)</span>  
 **:**   `'+'`   
 **|**   `'-'`   
 **;** 

<h4 id="multiplicativeOperator">multiplicativeOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[multiplicativeExpression](#multiplicativeExpression)<span style="color:gray">)</span>  
 **:**   `'*'`   
 **|**   `'/'`   
 **|**   `'%'`   
 **;** 

<h4 id="asOperator">asOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[asExpression](#asExpression)<span style="color:gray">)</span>  
 **:**   `'as'`   
 **|**   `'as?'`   
 **;** 

<h4 id="prefixUnaryOperator">prefixUnaryOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[unaryPrefix](#unaryPrefix)<span style="color:gray">)</span>  
 **:**   `'++'`   
 **|**   `'--'`   
 **|**   `'-'`   
 **|**   `'+'`   
 **|**  [excl](#excl)  
 **;** 

<h4 id="postfixUnaryOperator">postfixUnaryOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[postfixUnarySuffix](#postfixUnarySuffix)<span style="color:gray">)</span>  
 **:**   `'++'`   
 **|**   `'--'`   
 **|**   `'!'`  [excl](#excl)  
 **;** 

<h4 id="excl">excl</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[prefixUnaryOperator](#prefixUnaryOperator)<span style="color:gray">, </span>[postfixUnaryOperator](#postfixUnaryOperator)<span style="color:gray">)</span>  
 **:**   `'!'`   
 **|**  [EXCL_WS](#EXCL_WS)  
 **;** 

<h4 id="memberAccessOperator">memberAccessOperator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[navigationSuffix](#navigationSuffix)<span style="color:gray">)</span>  
 **:**   `'.'`   
 **|**  [safeNav](#safeNav)  
 **|**   `'::'`   
 **;** 

<h4 id="safeNav">safeNav</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[memberAccessOperator](#memberAccessOperator)<span style="color:gray">)</span>  
 **:**   `'?'`   `'.'`   
 **;** 

### Modifiers


<h4 id="modifiers">modifiers</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeAlias](#typeAlias)<span style="color:gray">, </span>[classDeclaration](#classDeclaration)<span style="color:gray">, </span>[primaryConstructor](#primaryConstructor)<span style="color:gray">, </span>[classParameter](#classParameter)<span style="color:gray">, </span>[companionObject](#companionObject)<span style="color:gray">, </span>[functionValueParameter](#functionValueParameter)<span style="color:gray">, </span>[functionDeclaration](#functionDeclaration)<span style="color:gray">, </span>[propertyDeclaration](#propertyDeclaration)<span style="color:gray">, </span>[getter](#getter)<span style="color:gray">, </span>[setter](#setter)<span style="color:gray">, </span>[objectDeclaration](#objectDeclaration)<span style="color:gray">, </span>[secondaryConstructor](#secondaryConstructor)<span style="color:gray">, </span>[enumEntry](#enumEntry)<span style="color:gray">)</span>  
 **:**  [annotation](#annotation)  
 **|**  [modifier](#modifier)**+**   
 **;** 

<h4 id="modifier">modifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[modifiers](#modifiers)<span style="color:gray">)</span>  
 **:**  [classModifier](#classModifier)  
 **|**  [memberModifier](#memberModifier)  
 **|**  [visibilityModifier](#visibilityModifier)  
 **|**  [functionModifier](#functionModifier)  
 **|**  [propertyModifier](#propertyModifier)  
 **|**  [inheritanceModifier](#inheritanceModifier)  
 **|**  [parameterModifier](#parameterModifier)  
 **|**  [platformModifier](#platformModifier)  
 **;** 

<h4 id="typeModifiers">typeModifiers</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[type](#type)<span style="color:gray">, </span>[receiverType](#receiverType)<span style="color:gray">)</span>  
 **:**  [typeModifier](#typeModifier)**+**   
 **;** 

<h4 id="typeModifier">typeModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeModifiers](#typeModifiers)<span style="color:gray">)</span>  
 **:**  [annotation](#annotation)  
 **|**   `'suspend'`   
 **;** 

<h4 id="classModifier">classModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[modifier](#modifier)<span style="color:gray">)</span>  
 **:**   `'enum'`   
 **|**   `'sealed'`   
 **|**   `'annotation'`   
 **|**   `'data'`   
 **|**   `'inner'`   
 **;** 

<h4 id="memberModifier">memberModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[modifier](#modifier)<span style="color:gray">)</span>  
 **:**   `'override'`   
 **|**   `'lateinit'`   
 **;** 

<h4 id="visibilityModifier">visibilityModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[modifier](#modifier)<span style="color:gray">)</span>  
 **:**   `'public'`   
 **|**   `'private'`   
 **|**   `'internal'`   
 **|**   `'protected'`   
 **;** 

<h4 id="varianceModifier">varianceModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeProjectionModifier](#typeProjectionModifier)<span style="color:gray">, </span>[typeParameterModifier](#typeParameterModifier)<span style="color:gray">)</span>  
 **:**   `'in'`   
 **|**   `'out'`   
 **;** 

<h4 id="typeParameterModifiers">typeParameterModifiers</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeParameter](#typeParameter)<span style="color:gray">)</span>  
 **:**  [typeParameterModifier](#typeParameterModifier)**+**   
 **;** 

<h4 id="typeParameterModifier">typeParameterModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeParameterModifiers](#typeParameterModifiers)<span style="color:gray">)</span>  
 **:**  [reificationModifier](#reificationModifier)  
 **|**  [varianceModifier](#varianceModifier)  
 **|**  [annotation](#annotation)  
 **;** 

<h4 id="functionModifier">functionModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[modifier](#modifier)<span style="color:gray">)</span>  
 **:**   `'tailrec'`   
 **|**   `'operator'`   
 **|**   `'infix'`   
 **|**   `'inline'`   
 **|**   `'external'`   
 **|**   `'suspend'`   
 **;** 

<h4 id="propertyModifier">propertyModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[modifier](#modifier)<span style="color:gray">)</span>  
 **:**   `'const'`   
 **;** 

<h4 id="inheritanceModifier">inheritanceModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[modifier](#modifier)<span style="color:gray">)</span>  
 **:**   `'abstract'`   
 **|**   `'final'`   
 **|**   `'open'`   
 **;** 

<h4 id="parameterModifier">parameterModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[setter](#setter)<span style="color:gray">, </span>[modifier](#modifier)<span style="color:gray">)</span>  
 **:**   `'vararg'`   
 **|**   `'noinline'`   
 **|**   `'crossinline'`   
 **;** 

<h4 id="reificationModifier">reificationModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[typeParameterModifier](#typeParameterModifier)<span style="color:gray">)</span>  
 **:**   `'reified'`   
 **;** 

<h4 id="platformModifier">platformModifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[modifier](#modifier)<span style="color:gray">)</span>  
 **:**   `'expect'`   
 **|**   `'actual'`   
 **;** 

### Annotations


<h4 id="annotation">annotation</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotatedDelegationSpecifier](#annotatedDelegationSpecifier)<span style="color:gray">, </span>[typeConstraint](#typeConstraint)<span style="color:gray">, </span>[variableDeclaration](#variableDeclaration)<span style="color:gray">, </span>[setter](#setter)<span style="color:gray">, </span>[typeProjectionModifier](#typeProjectionModifier)<span style="color:gray">, </span>[statement](#statement)<span style="color:gray">, </span>[forStatement](#forStatement)<span style="color:gray">, </span>[unaryPrefix](#unaryPrefix)<span style="color:gray">, </span>[annotatedLambda](#annotatedLambda)<span style="color:gray">, </span>[valueArgument](#valueArgument)<span style="color:gray">, </span>[whenSubject](#whenSubject)<span style="color:gray">, </span>[catchBlock](#catchBlock)<span style="color:gray">, </span>[modifiers](#modifiers)<span style="color:gray">, </span>[typeModifier](#typeModifier)<span style="color:gray">, </span>[typeParameterModifier](#typeParameterModifier)<span style="color:gray">)</span>  
 **:**  [singleAnnotation](#singleAnnotation)  
 **|**  [multiAnnotation](#multiAnnotation)  
 **;** 

<h4 id="singleAnnotation">singleAnnotation</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotation](#annotation)<span style="color:gray">)</span>  
 **:**  [annotationUseSiteTarget](#annotationUseSiteTarget) [unescapedAnnotation](#unescapedAnnotation)  
 **|**   `'@'`  [unescapedAnnotation](#unescapedAnnotation)  
 **;** 

<h4 id="multiAnnotation">multiAnnotation</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotation](#annotation)<span style="color:gray">)</span>  
 **:**  [annotationUseSiteTarget](#annotationUseSiteTarget)  `'['`  [unescapedAnnotation](#unescapedAnnotation)**+**   `']'`   
 **|**   `'@'`   `'['`  [unescapedAnnotation](#unescapedAnnotation)**+**   `']'`   
 **;** 

<h4 id="annotationUseSiteTarget">annotationUseSiteTarget</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[singleAnnotation](#singleAnnotation)<span style="color:gray">, </span>[multiAnnotation](#multiAnnotation)<span style="color:gray">)</span>  
 **:**  [ANNOTATION_USE_SITE_TARGET_FIELD](#ANNOTATION_USE_SITE_TARGET_FIELD)  
 **|**  [ANNOTATION_USE_SITE_TARGET_PROPERTY](#ANNOTATION_USE_SITE_TARGET_PROPERTY)  
 **|**  [ANNOTATION_USE_SITE_TARGET_GET](#ANNOTATION_USE_SITE_TARGET_GET)  
 **|**  [ANNOTATION_USE_SITE_TARGET_SET](#ANNOTATION_USE_SITE_TARGET_SET)  
 **|**  [ANNOTATION_USE_SITE_TARGET_RECEIVER](#ANNOTATION_USE_SITE_TARGET_RECEIVER)  
 **|**  [ANNOTATION_USE_SITE_TARGET_PARAM](#ANNOTATION_USE_SITE_TARGET_PARAM)  
 **|**  [ANNOTATION_USE_SITE_TARGET_SETPARAM](#ANNOTATION_USE_SITE_TARGET_SETPARAM)  
 **|**  [ANNOTATION_USE_SITE_TARGET_DELEGATE](#ANNOTATION_USE_SITE_TARGET_DELEGATE)  
 **;** 

<h4 id="unescapedAnnotation">unescapedAnnotation</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[fileAnnotation](#fileAnnotation)<span style="color:gray">, </span>[singleAnnotation](#singleAnnotation)<span style="color:gray">, </span>[multiAnnotation](#multiAnnotation)<span style="color:gray">)</span>  
 **:**  [constructorInvocation](#constructorInvocation)  
 **|**  [userType](#userType)  
 **;** 

### Identifiers


<h4 id="simpleIdentifier">simpleIdentifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[importAlias](#importAlias)<span style="color:gray">, </span>[typeAlias](#typeAlias)<span style="color:gray">, </span>[classDeclaration](#classDeclaration)<span style="color:gray">, </span>[classParameter](#classParameter)<span style="color:gray">, </span>[typeParameter](#typeParameter)<span style="color:gray">, </span>[typeConstraint](#typeConstraint)<span style="color:gray">, </span>[companionObject](#companionObject)<span style="color:gray">, </span>[functionDeclaration](#functionDeclaration)<span style="color:gray">, </span>[variableDeclaration](#variableDeclaration)<span style="color:gray">, </span>[setterParameter](#setterParameter)<span style="color:gray">, </span>[parameter](#parameter)<span style="color:gray">, </span>[objectDeclaration](#objectDeclaration)<span style="color:gray">, </span>[enumEntry](#enumEntry)<span style="color:gray">, </span>[simpleUserType](#simpleUserType)<span style="color:gray">, </span>[infixFunctionCall](#infixFunctionCall)<span style="color:gray">, </span>[directlyAssignableExpression](#directlyAssignableExpression)<span style="color:gray">, </span>[navigationSuffix](#navigationSuffix)<span style="color:gray">, </span>[valueArgument](#valueArgument)<span style="color:gray">, </span>[primaryExpression](#primaryExpression)<span style="color:gray">, </span>[superExpression](#superExpression)<span style="color:gray">, </span>[catchBlock](#catchBlock)<span style="color:gray">, </span>[callableReference](#callableReference)<span style="color:gray">, </span>[identifier](#identifier)<span style="color:gray">)</span>  
 **:**  [Identifier](#Identifier)  
 **|**   `'abstract'`   
 **|**   `'annotation'`   
 **|**   `'by'`   
 **|**   `'catch'`   
 **|**   `'companion'`   
 **|**   `'constructor'`   
 **|**   `'crossinline'`   
 **|**   `'data'`   
 **|**   `'dynamic'`   
 **|**   `'enum'`   
 **|**   `'external'`   
 **|**   `'final'`   
 **|**   `'finally'`   
 **|**   `'get'`   
 **|**   `'import'`   
 **|**   `'infix'`   
 **|**   `'init'`   
 **|**   `'inline'`   
 **|**   `'inner'`   
 **|**   `'internal'`   
 **|**   `'lateinit'`   
 **|**   `'noinline'`   
 **|**   `'open'`   
 **|**   `'operator'`   
 **|**   `'out'`   
 **|**   `'override'`   
 **|**   `'private'`   
 **|**   `'protected'`   
 **|**   `'public'`   
 **|**   `'reified'`   
 **|**   `'sealed'`   
 **|**   `'tailrec'`   
 **|**   `'set'`   
 **|**   `'vararg'`   
 **|**   `'where'`   
 **|**   `'expect'`   
 **|**   `'actual'`   
 **|**   `'const'`   
 **|**   `'suspend'`   
 **;** 

<h4 id="identifier">identifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[packageHeader](#packageHeader)<span style="color:gray">, </span>[importHeader](#importHeader)<span style="color:gray">)</span>  
 **:**  [simpleIdentifier](#simpleIdentifier) **(**  `'.'`  [simpleIdentifier](#simpleIdentifier)**)** **\***   
 **;** 

## Lexical grammar

### General


<h4 id="ShebangLine">ShebangLine</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[shebangLine](#shebangLine)<span style="color:gray">)</span>  
 **:**   `'#!'`  **~**  `[\r\n]` **\***   
 **;** 

<h4 id="DelimitedComment">DelimitedComment</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[DelimitedComment](#DelimitedComment)<span style="color:gray">, </span>[Hidden](#Hidden)<span style="color:gray">)</span>  
 **:**  **(**  `'/*'`  **(** [DelimitedComment](#DelimitedComment) **|**   `.` **)** **\*** **?**   `'*/'` **)**    
 **;** 

<h4 id="LineComment">LineComment</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[Hidden](#Hidden)<span style="color:gray">)</span>  
 **:**  **(**  `'//'`  **~**  `[\r\n]` **\*** **)**    
 **;** 

<h4 id="WS">WS</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[Hidden](#Hidden)<span style="color:gray">)</span>  
 **:**   `[\u0020\u0009\u000C]`    
 **;** 


helper  
<h4 id="Hidden">Hidden</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[EXCL_WS](#EXCL_WS)<span style="color:gray">, </span>[QUEST_WS](#QUEST_WS)<span style="color:gray">, </span>[NOT_IS](#NOT_IS)<span style="color:gray">, </span>[NOT_IN](#NOT_IN)<span style="color:gray">)</span>  
 **:**  [DelimitedComment](#DelimitedComment)  
 **|**  [LineComment](#LineComment)  
 **|**  [WS](#WS)  
 **;** 

### Separators and operations


<h4 id="RESERVED">RESERVED</h4>

 **:**   `'...'`   
 **;** 

<h4 id="EXCL_WS">EXCL_WS</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[excl](#excl)<span style="color:gray">)</span>  
 **:**   `'!'`  [Hidden](#Hidden)  
 **;** 

<h4 id="DOUBLE_ARROW">DOUBLE_ARROW</h4>

 **:**   `'=>'`   
 **;** 

<h4 id="DOUBLE_SEMICOLON">DOUBLE_SEMICOLON</h4>

 **:**   `';;'`   
 **;** 

<h4 id="HASH">HASH</h4>

 **:**   `'#'`   
 **;** 

<h4 id="QUEST_WS">QUEST_WS</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[quest](#quest)<span style="color:gray">)</span>  
 **:**   `'?'`  [Hidden](#Hidden)  
 **;** 

<h4 id="SINGLE_QUOTE">SINGLE_QUOTE</h4>

 **:**   `'\''`   
 **;** 

### Keywords


<h4 id="RETURN_AT">RETURN_AT</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[jumpExpression](#jumpExpression)<span style="color:gray">)</span>  
 **:**   `'return@'`  [Identifier](#Identifier)  
 **;** 

<h4 id="CONTINUE_AT">CONTINUE_AT</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[jumpExpression](#jumpExpression)<span style="color:gray">)</span>  
 **:**   `'continue@'`  [Identifier](#Identifier)  
 **;** 

<h4 id="BREAK_AT">BREAK_AT</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[jumpExpression](#jumpExpression)<span style="color:gray">)</span>  
 **:**   `'break@'`  [Identifier](#Identifier)  
 **;** 

<h4 id="THIS_AT">THIS_AT</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[thisExpression](#thisExpression)<span style="color:gray">)</span>  
 **:**   `'this@'`  [Identifier](#Identifier)  
 **;** 

<h4 id="SUPER_AT">SUPER_AT</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[superExpression](#superExpression)<span style="color:gray">)</span>  
 **:**   `'super@'`  [Identifier](#Identifier)  
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_FILE">ANNOTATION_USE_SITE_TARGET_FILE</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[fileAnnotation](#fileAnnotation)<span style="color:gray">)</span>  
 **:**   `'@file'`   `':'`   
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_FIELD">ANNOTATION_USE_SITE_TARGET_FIELD</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotationUseSiteTarget](#annotationUseSiteTarget)<span style="color:gray">)</span>  
 **:**   `'@field'`   `':'`   
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_PROPERTY">ANNOTATION_USE_SITE_TARGET_PROPERTY</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotationUseSiteTarget](#annotationUseSiteTarget)<span style="color:gray">)</span>  
 **:**   `'@property'`   `':'`   
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_GET">ANNOTATION_USE_SITE_TARGET_GET</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotationUseSiteTarget](#annotationUseSiteTarget)<span style="color:gray">)</span>  
 **:**   `'@get'`   `':'`   
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_SET">ANNOTATION_USE_SITE_TARGET_SET</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotationUseSiteTarget](#annotationUseSiteTarget)<span style="color:gray">)</span>  
 **:**   `'@set'`   `':'`   
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_RECEIVER">ANNOTATION_USE_SITE_TARGET_RECEIVER</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotationUseSiteTarget](#annotationUseSiteTarget)<span style="color:gray">)</span>  
 **:**   `'@receiver'`   `':'`   
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_PARAM">ANNOTATION_USE_SITE_TARGET_PARAM</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotationUseSiteTarget](#annotationUseSiteTarget)<span style="color:gray">)</span>  
 **:**   `'@param'`   `':'`   
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_SETPARAM">ANNOTATION_USE_SITE_TARGET_SETPARAM</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotationUseSiteTarget](#annotationUseSiteTarget)<span style="color:gray">)</span>  
 **:**   `'@setparam'`   `':'`   
 **;** 

<h4 id="ANNOTATION_USE_SITE_TARGET_DELEGATE">ANNOTATION_USE_SITE_TARGET_DELEGATE</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[annotationUseSiteTarget](#annotationUseSiteTarget)<span style="color:gray">)</span>  
 **:**   `'@delegate'`   `':'`   
 **;** 

<h4 id="TYPEOF">TYPEOF</h4>

 **:**   `'typeof'`   
 **;** 

<h4 id="NOT_IS">NOT_IS</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[isOperator](#isOperator)<span style="color:gray">)</span>  
 **:**   `'!is'`  [Hidden](#Hidden)  
 **;** 

<h4 id="NOT_IN">NOT_IN</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[inOperator](#inOperator)<span style="color:gray">)</span>  
 **:**   `'!in'`  [Hidden](#Hidden)  
 **;** 

### Literals



helper  
<h4 id="DecDigit">DecDigit</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[DecDigitOrSeparator](#DecDigitOrSeparator)<span style="color:gray">, </span>[DecDigits](#DecDigits)<span style="color:gray">, </span>[IntegerLiteral](#IntegerLiteral)<span style="color:gray">)</span>  
 **:**   `'0'`  `..`  `'9'`   
 **;** 


helper  
<h4 id="DecDigitNoZero">DecDigitNoZero</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[IntegerLiteral](#IntegerLiteral)<span style="color:gray">)</span>  
 **:**   `'1'`  `..`  `'9'`   
 **;** 


helper  
<h4 id="DecDigitOrSeparator">DecDigitOrSeparator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[DecDigits](#DecDigits)<span style="color:gray">, </span>[IntegerLiteral](#IntegerLiteral)<span style="color:gray">)</span>  
 **:**  [DecDigit](#DecDigit)  
 **|**   `'_'`   
 **;** 


helper  
<h4 id="DecDigits">DecDigits</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[DoubleExponent](#DoubleExponent)<span style="color:gray">, </span>[FloatLiteral](#FloatLiteral)<span style="color:gray">, </span>[DoubleLiteral](#DoubleLiteral)<span style="color:gray">)</span>  
 **:**  [DecDigit](#DecDigit) [DecDigitOrSeparator](#DecDigitOrSeparator)**\***  [DecDigit](#DecDigit)  
 **|**  [DecDigit](#DecDigit)  
 **;** 


helper  
<h4 id="DoubleExponent">DoubleExponent</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[DoubleLiteral](#DoubleLiteral)<span style="color:gray">)</span>  
 **:**   `[eE]`   `[+-]` **?**  [DecDigits](#DecDigits)  
 **;** 

<h4 id="RealLiteral">RealLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[literalConstant](#literalConstant)<span style="color:gray">)</span>  
 **:**  [FloatLiteral](#FloatLiteral)  
 **|**  [DoubleLiteral](#DoubleLiteral)  
 **;** 

<h4 id="FloatLiteral">FloatLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[RealLiteral](#RealLiteral)<span style="color:gray">)</span>  
 **:**  [DoubleLiteral](#DoubleLiteral)  `[fF]`   
 **|**  [DecDigits](#DecDigits)  `[fF]`   
 **;** 

<h4 id="DoubleLiteral">DoubleLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[RealLiteral](#RealLiteral)<span style="color:gray">, </span>[FloatLiteral](#FloatLiteral)<span style="color:gray">)</span>  
 **:**  [DecDigits](#DecDigits)**?**   `'.'`  [DecDigits](#DecDigits) [DoubleExponent](#DoubleExponent)**?**   
 **|**  [DecDigits](#DecDigits) [DoubleExponent](#DoubleExponent)  
 **;** 

<h4 id="IntegerLiteral">IntegerLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[literalConstant](#literalConstant)<span style="color:gray">, </span>[UnsignedLiteral](#UnsignedLiteral)<span style="color:gray">, </span>[LongLiteral](#LongLiteral)<span style="color:gray">)</span>  
 **:**  [DecDigitNoZero](#DecDigitNoZero) [DecDigitOrSeparator](#DecDigitOrSeparator)**\***  [DecDigit](#DecDigit)  
 **|**  [DecDigit](#DecDigit)  
 **;** 


helper  
<h4 id="HexDigit">HexDigit</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[HexDigitOrSeparator](#HexDigitOrSeparator)<span style="color:gray">, </span>[HexLiteral](#HexLiteral)<span style="color:gray">, </span>[UniCharacterLiteral](#UniCharacterLiteral)<span style="color:gray">)</span>  
 **:**   `[0-9a-fA-F]`   
 **;** 


helper  
<h4 id="HexDigitOrSeparator">HexDigitOrSeparator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[HexLiteral](#HexLiteral)<span style="color:gray">)</span>  
 **:**  [HexDigit](#HexDigit)  
 **|**   `'_'`   
 **;** 

<h4 id="HexLiteral">HexLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[literalConstant](#literalConstant)<span style="color:gray">, </span>[UnsignedLiteral](#UnsignedLiteral)<span style="color:gray">, </span>[LongLiteral](#LongLiteral)<span style="color:gray">)</span>  
 **:**   `'0'`   `[xX]`  [HexDigit](#HexDigit) [HexDigitOrSeparator](#HexDigitOrSeparator)**\***  [HexDigit](#HexDigit)  
 **|**   `'0'`   `[xX]`  [HexDigit](#HexDigit)  
 **;** 


helper  
<h4 id="BinDigit">BinDigit</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[BinDigitOrSeparator](#BinDigitOrSeparator)<span style="color:gray">, </span>[BinLiteral](#BinLiteral)<span style="color:gray">)</span>  
 **:**   `[01]`   
 **;** 


helper  
<h4 id="BinDigitOrSeparator">BinDigitOrSeparator</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[BinLiteral](#BinLiteral)<span style="color:gray">)</span>  
 **:**  [BinDigit](#BinDigit)  
 **|**   `'_'`   
 **;** 

<h4 id="BinLiteral">BinLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[literalConstant](#literalConstant)<span style="color:gray">, </span>[UnsignedLiteral](#UnsignedLiteral)<span style="color:gray">, </span>[LongLiteral](#LongLiteral)<span style="color:gray">)</span>  
 **:**   `'0'`   `[bB]`  [BinDigit](#BinDigit) [BinDigitOrSeparator](#BinDigitOrSeparator)**\***  [BinDigit](#BinDigit)  
 **|**   `'0'`   `[bB]`  [BinDigit](#BinDigit)  
 **;** 

<h4 id="UnsignedLiteral">UnsignedLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[literalConstant](#literalConstant)<span style="color:gray">)</span>  
 **:**  **(** [IntegerLiteral](#IntegerLiteral) **|**  [HexLiteral](#HexLiteral) **|**  [BinLiteral](#BinLiteral)**)**   `[uU]`   `'L'` **?**   
 **;** 

<h4 id="LongLiteral">LongLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[literalConstant](#literalConstant)<span style="color:gray">)</span>  
 **:**  **(** [IntegerLiteral](#IntegerLiteral) **|**  [HexLiteral](#HexLiteral) **|**  [BinLiteral](#BinLiteral)**)**   `'L'`   
 **;** 

<h4 id="BooleanLiteral">BooleanLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[literalConstant](#literalConstant)<span style="color:gray">)</span>  
 **:**   `'true'`   
 **|**   `'false'`   
 **;** 

<h4 id="CharacterLiteral">CharacterLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[literalConstant](#literalConstant)<span style="color:gray">)</span>  
 **:**   `'\''`  **(** [EscapeSeq](#EscapeSeq) **|**  **~**  `[\n\r'\\]` **)**   `'\''`   
 **;** 

### Identifiers



helper  
<h4 id="UnicodeDigit">UnicodeDigit</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[Identifier](#Identifier)<span style="color:gray">)</span>  
 **:**  **<a href="https://github.com/Kotlin/kotlin-spec/blob/master/grammar/src/main/antlr/UnicodeClasses.g4#L1605" target="_black">UNICODE_CLASS_ND</a>** 
 **;** 

<h4 id="Identifier">Identifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[simpleIdentifier](#simpleIdentifier)<span style="color:gray">, </span>[RETURN_AT](#RETURN_AT)<span style="color:gray">, </span>[CONTINUE_AT](#CONTINUE_AT)<span style="color:gray">, </span>[BREAK_AT](#BREAK_AT)<span style="color:gray">, </span>[THIS_AT](#THIS_AT)<span style="color:gray">, </span>[SUPER_AT](#SUPER_AT)<span style="color:gray">, </span>[IdentifierOrSoftKey](#IdentifierOrSoftKey)<span style="color:gray">)</span>  
 **:**  **(** [Letter](#Letter) **|**   `'_'` **)**  **(** [Letter](#Letter) **|**   `'_'`  **|**  [UnicodeDigit](#UnicodeDigit)**)** **\***   
 **|**   ``'`'``  **~** **(**  `[\r\n]`  **|**   ``'`'``  **|**   `'.'`  **|**   `';'`  **|**   `':'`  **|**   `'\\'`  **|**   `'/'`  **|**   `'['`  **|**   `']'`  **|**   `'<'`  **|**   `'>'` **)** **+**   ``'`'``   
 **;** 

Depending on the target and publicity of the declaration, the set of allowed symbols in identifiers is different.
This rule contains the union of allowed symbols from all targets.
Thus, the code for any target can be parsed using the grammar.

The allowed symbols in identifiers corresponding to the target and publicity of the declaration are given below.

##### Kotlin/JVM (any declaration publicity)

 `~`
 **(**
 `[\r\n]`
 **|**
 ``'`'``
 **|**
 `'.'`
 **|**
 `';'`
 **|**
 `':'`
 **|**
 `'\'`
 **|**
 `'/'`
 **|**
 `'['`
 **|**
 `']'`
 **|**
 `'<'`
 **|**
 `'>'`
 **)**

##### Kotlin/Android (any declaration publicity)

The allowed symbols are different from allowed symbols for Kotlin/JVM and correspond to the <a href="https://source.android.com/devices/tech/dalvik/dex-format#simplename" target="_blank">Dalvik Executable format</a>.

##### Kotlin/JS (private declarations)

 `~`
 **(**
 `[\r\n]`
 **|**
 ``'`'``
 **)**

##### Kotlin/JS (public declarations)

The allowed symbols for public declarations correspond to the <a href="https://www.ecma-international.org/ecma-262/5.1/#sec-7.6" target="_blank">ECMA specification (section 7.6)</a> except that ECMA reserved words is allowed.

##### Kotlin/Native (any declaration publicity)

 `~`
 **(**
 `[\r\n]`
 **|**
 ``'`'``
 **)**


<h4 id="IdentifierOrSoftKey">IdentifierOrSoftKey</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[IdentifierAt](#IdentifierAt)<span style="color:gray">, </span>[FieldIdentifier](#FieldIdentifier)<span style="color:gray">)</span>  
 **:**  [Identifier](#Identifier)  
 **|**   `'abstract'`   
 **|**   `'annotation'`   
 **|**   `'by'`   
 **|**   `'catch'`   
 **|**   `'companion'`   
 **|**   `'constructor'`   
 **|**   `'crossinline'`   
 **|**   `'data'`   
 **|**   `'dynamic'`   
 **|**   `'enum'`   
 **|**   `'external'`   
 **|**   `'final'`   
 **|**   `'finally'`   
 **|**   `'get'`   
 **|**   `'import'`   
 **|**   `'infix'`   
 **|**   `'init'`   
 **|**   `'inline'`   
 **|**   `'inner'`   
 **|**   `'internal'`   
 **|**   `'lateinit'`   
 **|**   `'noinline'`   
 **|**   `'open'`   
 **|**   `'operator'`   
 **|**   `'out'`   
 **|**   `'override'`   
 **|**   `'private'`   
 **|**   `'protected'`   
 **|**   `'public'`   
 **|**   `'reified'`   
 **|**   `'sealed'`   
 **|**   `'tailrec'`   
 **|**   `'set'`   
 **|**   `'vararg'`   
 **|**   `'where'`   
 **|**   `'expect'`   
 **|**   `'actual'`   
 **|**   `'const'`   
 **|**   `'suspend'`   
 **;** 

<h4 id="IdentifierAt">IdentifierAt</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[label](#label)<span style="color:gray">)</span>  
 **:**  [IdentifierOrSoftKey](#IdentifierOrSoftKey)  `'@'`   
 **;** 

<h4 id="FieldIdentifier">FieldIdentifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[LineStrRef](#LineStrRef)<span style="color:gray">, </span>[MultiLineStrRef](#MultiLineStrRef)<span style="color:gray">)</span>  
 **:**   `'$'`  [IdentifierOrSoftKey](#IdentifierOrSoftKey)  
 **;** 


helper  
<h4 id="UniCharacterLiteral">UniCharacterLiteral</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[EscapeSeq](#EscapeSeq)<span style="color:gray">, </span>[LineStrEscapedChar](#LineStrEscapedChar)<span style="color:gray">)</span>  
 **:**   `'\\'`   `'u'`  [HexDigit](#HexDigit) [HexDigit](#HexDigit) [HexDigit](#HexDigit) [HexDigit](#HexDigit)  
 **;** 


helper  
<h4 id="EscapedIdentifier">EscapedIdentifier</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[EscapeSeq](#EscapeSeq)<span style="color:gray">, </span>[LineStrEscapedChar](#LineStrEscapedChar)<span style="color:gray">)</span>  
 **:**   `'\\'`  **(**  `'t'`  **|**   `'b'`  **|**   `'r'`  **|**   `'n'`  **|**   `'\''`  **|**   `'"'`  **|**   `'\\'`  **|**   `'$'` **)**   
 **;** 


helper  
<h4 id="EscapeSeq">EscapeSeq</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[CharacterLiteral](#CharacterLiteral)<span style="color:gray">)</span>  
 **:**  [UniCharacterLiteral](#UniCharacterLiteral)  
 **|**  [EscapedIdentifier](#EscapedIdentifier)  
 **;** 

### Characters



helper  
<h4 id="Letter">Letter</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[Identifier](#Identifier)<span style="color:gray">)</span>  
 **:**  **<a href="https://github.com/Kotlin/kotlin-spec/blob/master/grammar/src/main/antlr/UnicodeClasses.g4#L9" target="_black">UNICODE_CLASS_LL</a>** 
 **|**  **<a href="https://github.com/Kotlin/kotlin-spec/blob/master/grammar/src/main/antlr/UnicodeClasses.g4#L613" target="_black">UNICODE_CLASS_LM</a>** 
 **|**  **<a href="https://github.com/Kotlin/kotlin-spec/blob/master/grammar/src/main/antlr/UnicodeClasses.g4#L673" target="_black">UNICODE_CLASS_LO</a>** 
 **|**  **<a href="https://github.com/Kotlin/kotlin-spec/blob/master/grammar/src/main/antlr/UnicodeClasses.g4#L999" target="_black">UNICODE_CLASS_LT</a>** 
 **|**  **<a href="https://github.com/Kotlin/kotlin-spec/blob/master/grammar/src/main/antlr/UnicodeClasses.g4#L1011" target="_black">UNICODE_CLASS_LU</a>** 
 **|**  **<a href="https://github.com/Kotlin/kotlin-spec/blob/master/grammar/src/main/antlr/UnicodeClasses.g4#L1642" target="_black">UNICODE_CLASS_NL</a>** 
 **;** 

### Strings


<h4 id="LineStrRef">LineStrRef</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[lineStringContent](#lineStringContent)<span style="color:gray">)</span>  
 **:**  [FieldIdentifier](#FieldIdentifier)  
 **;** 

See [String templates](basic-types.html#string-templates)


<h4 id="LineStrText">LineStrText</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[lineStringContent](#lineStringContent)<span style="color:gray">)</span>  
 **:**  **~** **(**  `'\\'`  **|**   `'"'`  **|**   `'$'` **)** **+**   
 **|**   `'$'`   
 **;** 

<h4 id="LineStrEscapedChar">LineStrEscapedChar</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[lineStringContent](#lineStringContent)<span style="color:gray">)</span>  
 **:**  [EscapedIdentifier](#EscapedIdentifier)  
 **|**  [UniCharacterLiteral](#UniCharacterLiteral)  
 **;** 

<h4 id="TRIPLE_QUOTE_CLOSE">TRIPLE_QUOTE_CLOSE</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[multiLineStringLiteral](#multiLineStringLiteral)<span style="color:gray">)</span>  
 **:**  **(**  `'"'` **?**   `'"""'` **)**    
 **;** 

<h4 id="MultiLineStrRef">MultiLineStrRef</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[multiLineStringContent](#multiLineStringContent)<span style="color:gray">)</span>  
 **:**  [FieldIdentifier](#FieldIdentifier)  
 **;** 

<h4 id="MultiLineStrText">MultiLineStrText</h4>

<span style="color:gray">(used by </span><span style="color:gray">, </span>[multiLineStringContent](#multiLineStringContent)<span style="color:gray">)</span>  
 **:**  **~** **(**  `'"'`  **|**   `'$'` **)** **+**   
 **|**   `'$'`   
 **;** 

<h4 id="ErrorCharacter">ErrorCharacter</h4>

 **:**   `.`   
 **;** 
