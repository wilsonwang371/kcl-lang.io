---
title: "KCL Errors and Warnings"
linkTitle: "KCL Errors and Warnings"
type: "docs"
weight: 2
description: KCL Errors and Warnings
---

The articles in this section of the documentation explain the diagnostic error and warning messages that are generated by the KCL.

**Important:**
**The KCL can report many kinds of errors and warnings. After an error or warning is found, the build tools may make assumptions about code intent and attempt to continue, so that more issues can be reported at the same time.  If the tools make the wrong assumption,  later errors or warnings may not apply to your project. When you correct issues in your project, always start with the first error or warning that's reported and rebuild often. One fix may make many subsequent errors go away.**

In the following sections you will find:

[KCL Syntax Error (E1xxx)](#11-kcl-syntax-error-e1xxx) : The KCL may reports KCL syntax errors when illegal syntax is used in KCL program.

[KCL Compile Error (E2xxx)](#12-kcl-compile-error-e2xxx): The KCL may reports KCL compile errors when the KCL program conforms to the KCL syntax but does not conform to the KCL semantics.

[KCL Runtime Error (E3xxx)](#13-kcl-runtime-error-e3xxx): The KCL may report KCL runtime errors when the virtual machine executing a KCL program that passes the compilation.

[KCL Compile Warning (W2xxx)](#14-kcl-compile-warning-w2xxx): When the compiler compiles KCL programs and finds possible potential errors, such warnings will be reported by KCL.

## 1.1 KCL Syntax Error (E1xxx)

This section mainly includes KCL errors:

| ewcode | KCL exception                                                       | messages                |
| ------ | ------------------------------------------------------------------- | ----------------------- |
| E1001  | [InvalidSyntaxError](#111-invalidsyntaxerror-e1001)                 | Invalid syntax          |
| E1002  | [KCLTabError](#112-kcltaberror-e1002)                               | Tab Error               |
| E1003  | [KCLIndentationError](#113-kclindentationerrore1003)                | Indentation Error       |
| E1I37  | [IllegalArgumentSyntaxError](#114-illegalargumentsyntaxerror-e1i37) | Illegal argument syntax |

### 1.1.1 InvalidSyntaxError (E1001)

KCL will report `InvalidSyntaxError` when KCL has a syntax error.

The `ewcode` of `InvalidSyntaxError` is `E1001`.

For example:

```python
a, b = 1, 2 # Multiple assign is illegal in KCL syntax
```

The KCL program will cause the following error message.

```shell
error[E1001]: InvalidSyntax
 --> /syntax_error/general/multiple_assign/case0/main.k:1:2
  |
1 | a, b = 1, 2 # Multiple assign is illegal in KCL syntax
  |  ^ expected statement
  |
```

Possible resolution:

- Check and fix KCL syntax errors based on the KCL Language Standard

### 1.1.2 KCLTabError

KCL will report `KCLTabError` when KCL has a tab and white space syntax error.

In KCL, it is forbidden to mix tabs and four spaces in one indentation block. And we recommend only using white spaces or tabs for indentation in the entire KCL project, don’t mix them.

For example:

```python
schema Person:
    name: str # begin with a tab
    age: int # begin with four white spaces, 
             # and four white spaces != tab in the env
```

The KCL program will cause the following error message.

```shell
error[E1001]: InvalidSyntax
 --> File /syntax_error/tab/tab_error_0/main.k:6:5
  |
3 |     age: int = 1
  |     ^ inconsistent use of tabs and spaces in indentation
  |
```

Possible resolution:

- Only use a tab or four white spaces in KCL, do not mix them.

### 1.1.3 KCLIndentationError

KCL will report `KCLIndentationError` when KCL has an indentation syntax error.

The KCL syntax includes indentation. A tab or four white spaces in KCL represents an indentation. The other cases will be regarded as syntax errors by KCL.

For example:

```python
schema Person:
    name: str # a tab or four white spaces is legal.
   age: int # three white spaces are illegal
  info: str # two white spaces is illegal
```

The KCL program will cause the following error message.

```shell
error[E1001]: InvalidSyntax
 --> /syntax_error/indent/indent_error_0/main.k:3:4
  |
3 |    age: int # three white spaces are illegal
  |    ^ unindent 3 does not match any outer indentation level
  |
```

Possible resolution:

- Only use a tab or four white spaces in the KCL program for indentation.

### 1.1.4 IllegalArgumentSyntaxError (E1I37)

KCL will report `IllegalArgumentSyntaxError` when KCL has an illegal argument in KCL syntax.

For example:

```python
# Parameters without default values 
# must be in front of parameters with default values.
a = option(type="list", default={"key": "value"}, "key1")
```

The KCL program will cause the following error message.

```shell
error[E1001]: InvalidSyntax
 --> /option/type_convert_fail_2/main.k:3:57
  |
3 | a = option(type="list", default={"key": "value"}, "key1")
  |                                                         ^ positional argument follows keyword argument
  |
```

Possible resolution:

```python
func(input_1, ..., input_n, param_with_key_1 = input_with_key_1, ..., param_with_key_n = input_with_key_n)
```

## 1.2 KCL Compile Error (E2xxx)

This section mainly includes KCL errors:

| ewcode | KCL exception                                                          | messages                                            |
| ------ | ---------------------------------------------------------------------- | --------------------------------------------------- |
| E2F04  | [CannotFindModule](#121-cannotfindmodulee2f04)                         | Cannot find the module                              |
| E2F05  | [FailedLoadModule](#122-failedloadmodulee2f05)                         | Failed to load module                               |
| E2H13  | [UnKnownDecoratorError](#123-unknowndecoratorerrore2h13)               | UnKnown decorator                                   |
| E2H14  | [InvalidDecoratorTargetError](#124-invaliddecoratortargeterrore2h14)   | Invalid Decorator Target                            |
| E2C15  | [MixinNamingError](#125-mixinnamingerrore2c15)                         | Illegal mixin naming                                |
| E2C16  | [MixinStructureIllegal](#126-mixinstructureillegale2c16)               | Illegal mixin structure                             |
| E2B17  | [CannotAddMembersComplieError](#127-cannotaddmemberscomplieerrore2b17) | Cannot add members to a schema                      |
| E2B20  | [IndexSignatureError](#128-indexsignatureerrore2b20)                   | Invalid index signature                             |
| E2G22  | [TypeComplieError](#129-typecomplieerrore2g22)                         | The type got is inconsistent with the type expected |
| E2L23  | [CompileError](#1210-compileerrore2l23)                                | A complie error occurs during compiling             |
| E2L25  | [KCLNameError](#1211-kclnameerrore2l25)                                | Name Error                                          |
| E2L26  | [KCLValueError](#1212-kclvalueerrore2l26)                              | Value Error                                         |
| E2L27  | [KCLKeyError](#1213-kclkeyerrore2l27)                                  | Key Error                                           |
| E2L28  | [UniqueKeyError](#1214-uniquekeyerrore2l28)                            | Unique key error                                    |
| E2A29  | [KCLAttributeComplieError](#1215-kclattributecomplieerrore2a29)        | Attribute error occurs during compiling             |
| E2D32  | [MultiInheritError](#1216-multiinheriterrore2d32)                      | Multiple inheritance is illegal                     |
| E2D34  | [IllegalInheritError](#1217-illegalinheriterrore2d34)                  | Illegal inheritance                                 |
| E2I36  | [IllegalArgumentComplieError](#1218-illegalargumentcomplieerrore2i36)  | Illegal argument during compiling                   |
| E3L41  | [ImmutableCompileError](#1219-immutablecompileerror-e3l41)             | Immutable variable is modified                      |

### 1.2.1 CannotFindModule(E2F04)

KCL will report `CannotFindModule` when KCL imports a module that does not exist.

The `ewcode` of `CannotFindModule` is `E2F04`.

For example:

```python
import .some0.pkg1 as some00  # some0 not found in package

Name1 = some00.Name  # some0.pkg1.name
```

The KCL program will cause the following error message.

```shell
error[E2F04]: CannotFindModule
 --> import_abs_fail_0/app-main/main.k:1:1
  |
1 | import .some0.pkg1 as some00  # some0 not found in package
  |  Cannot find the module .some0.pkg1
  |
```

Possible resolution:

- Add the import module file under the import path.

### 1.2.2 FailedLoadModule(E2F05)

KCL will report `FailedLoadModule` when an error occurs during loading a KCL external package.

The `ewcode` of `FailedLoadModule` is `E2F05`.

Possible resolution:

- Check whether the module file is readable.
- Check whether the module file is a KCL file.

### 1.2.3 UnKnownDecoratorError(E2H13)

KCL will report `UnKnownDecoratorError` when an unknown decorator is used in KCL.

The `ewcode` of `UnKnownDecoratorError` is `E2H13`.

For example:

```python
@err_deprecated # It is an unknown decorator
schema Person:
    firstName: str = "John"
    lastName: str
    name: str

JohnDoe = Person {
    name: "deprecated"
}
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> deprecated/unknown_fail_1/main.k:1:2
  |
1 | @err_deprecated # 这是一个非法的装饰器
  |  ^ UnKnown decorator err_deprecated
  |
```

Possible resolution:

- Check whether the decorator exists.

### 1.2.4 InvalidDecoratorTargetError(E2H14)

KCL will report `InvalidDecoratorTargetError` when the target cannot be the target of the decorator.

The `ewcode` of `InvalidDecoratorTargetError` is `E2H14`.

Possible resolution:

- Check whether the decorator in KCL is illegal.

### 1.2.5 MixinNamingError(E2C15)

KCL will report `MixinNamingError` when a mixin name does not end with 'Mixin'.

The `ewcode` of `MixinNamingError` is `E2C15`.

For example:

```python
schema Person:
    firstName: str
    lastName: str
    fullName: str

schema Fullname: # It is a mixin, but 'Fullname' is not end with 'Mixin
    fullName = "{} {}".format(firstName, lastName)

schema Scholar(Person):
    mixin [Fullname]
    school: str

JohnDoe = Scholar {
    "firstName": "John",
    "lastName": "Doe",
    "fullName": "Doe Jon"
}
```

The KCL program will cause the following error message.

```shell
error[E2D34]: IllegalInheritError
  --> mixin/invalid_name_failure/main.k:10:12
   |
10 |     mixin [Fullname]
   |            ^ illegal schema mixin object type 'Fullname'
   |
```

Possible resolution:

- If the schema is a mixin, then the name of the schema should end with Mixin.

### 1.2.6 MixinStructureIllegal(E2C16)

KCL will report `MixinStructureIllegal` when the KCL structure is illegal.

The `ewcode` of `MixinStructureIllegal` is `E2C16`.

Possible resolution:

- Check the structure of schema as Mixin.

### 1.2.7 CannotAddMembersComplieError(E2B17)

KCL will report `CannotAddMembersComplieError` when members that are not in the schema are used.

The `ewcode` of `CannotAddMembersComplieError` is `E2B17`.

For example:

```python
schema Girl:
    gender: str = "female"

alice = Girl {
    "first": "alice", # "first" can not be found in schema Girl
    "last": " Green", # "last" can not be found in schema Girl
    "age": 10  # "age" can not be found in schema Girl
}
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> /invalid/add_attribute/main.k:5:5
  |
5 |     "first": "alice",
  |     ^ Cannot add member 'first' to schema 'Girl'
  |

error[E2L23]: CompileError
 --> /invalid/add_attribute/main.k:6:5
  |
6 |     "last": " Green",
  |     ^ Cannot add member 'last' to schema 'Girl'
  |

error[E2L23]: CompileError
 --> /invalid/add_attribute/main.k:7:5
  |
7 |     "age": 10
  |     ^ Cannot add member 'age' to schema 'Girl'
  |
```

Possible resolution:

- Add the members to the schema.
- Remove the using of the members not exists

### 1.2.8 IndexSignatureError(E2B20)

The `ewcode` of `IndexSignatureError` is `E2B20`.

KCL will report `IndexSignatureError` when:

1. Multiple index signatures in one schema.

For example:

```python
schema Data:
    [str]: str
    [str]: int # Multiple index signatures in one schema.

data = Data {
    name: "test"
}
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
---> index_signature/fail_1/main.k:3:5
  |
3 |    [str]: int
  |  5 ^ only one index signature is allowed in the schema
  |
```

Possible resolution:

- Remove the extra index signature in the schema.

2. The name of index signature attributes has the same name that conflicts with other attributes in the schema.

For example:

```python
schema Data:
    name: str  # name
    [name: str]: str # the same name with the above attribute

data = Data {
    name: "test"
}
```

The KCL program will cause the following error message.

```shell
error[E1001]: IndexSignatureError
 --> index_signature/fail_2/main.k:3:5
  |
3 |     [name: str]: str # the same name with the above attribute
  |     ^ index signature attribute name 'name' cannot have the same name as schema attributes
  |
```

Possible resolution:

- Remove attributes or index signatures that have conflicts with the same name in the schema, or change their names.

3. Schema index signature value type has conflicts with the instance of schema.

For example:

```python
schema Data:
    [str]: int

data = Data {
    name: "test" # Conflict with [str]:int, "test" is a string.
}
```

The KCL program will cause the following error message.

```shell
error[E2G22]: TypeError
 --> index_signature/fail_3/main.k:5:5
  |
5 |     name: "test" # Conflict with [str]:int, "test" is a string.
  |     ^ expected int, got str(test)
  |
```

Possible resolution:

- Check that the type of schema index signature is consistent with the attribute type in the schema instance.

4. Schema index signature has conflicts with schema.

For example:

```python
schema Data:
    count: int # got int 
    [str]: str # except str

data = Data {
    count: 1 
}
```

The KCL program will cause the following error message.

```shell
error[E1001]: IndexSignatureError
 --> index_signature/fail_4/main.k:2:5
  |
2 |     count: int
  |     ^ the type 'int' of schema attribute 'count' does not meet the index signature definition [str]: str
  |
```

Possible resolution:

- Change schema for index signature or change index signature for schema.

### 1.2.9 TypeComplieError(E2G22)

KCL will report `TypeComplieError` when a type error occurs in compiling type check.

The `ewcode` of `TypeComplieError` is `E2G22`.

For example:

```python
schema Person:
    firstName: str
    lastName: int

JohnDoe = Person {
    "firstName": "John",
    "lastName": "Doe" # Type Error，lastName: int，“Doe” is a string.
}
```

The KCL program will cause the following error message.

```shell
error[E2G22]: TypeError
 --> type/type_fail_0/main.k:7:5
  |
7 |     "lastName": "Doe" # Type Error，lastName: int，“Doe” is a string.
  |     ^ expected int, got str(Doe)
  |

 --> type/type_fail_0/main.k:3:5
  |
3 |     lastName: int
  |     ^ variable is defined here, its type is int, but got str(Doe)
  |
```

Possible resolution:

- Check that the type of value assigned to a variable is consistent with the type of the variable.

### 1.2.10 CompileError(E2L23)

The `ewcode` of `CompileError` is `E2L23`.

KCL will report `CompileError` when:

1. unsupport type union.

For example:

```python
_data = [1, 2, 3]
_data |= "value"
```

The KCL program will cause the following error message.

```shell
error[E2G22]: TypeError
 --> union/fail/fail_1/main.k:2:1
  |
2 | _data |= "value"
  | ^ unsupported operand type(s) for |: '[int]' and 'str(value)'
  |
```

Possible resolution:

1. unsupported operand type.

For example:

```python
a = None
b = 1 + None # Unsupport operand type + for int and None
```

The KCL program will cause the following error message.

```shell
error[E2G22]: TypeError
 --> operator/operator_fail_0/main.k:2:5
  |
2 | b = 1 + None # Unsupport operand type + for int and None
  |     ^ unsupported operand type(s) for +: 'int(1)' and 'NoneType'
  |
```

Possible resolution:

- Adjust the operator so that it supports both operand types.
- Adjust the operands so that they conform to the constraints of the operator at the same time.

1. variable is not defined.

For example:

```python
a = 1
b = "${c + 1}" # 'c' is not defined
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> var_not_define_fail_0/main.k:2:8
  |
2 | b = "${c + 1}" # 'c' is not defined
  |        ^ name 'c' is not defined
  |
```

Possible resolution:

- Define undefined variables.
- Remove the undefined variable from the expression.

1. invalid assign expression.

For example:

```python
# pkg.k
a = 1

# main.k
import pkg
pkg.a |= 2
```

The KCL program will cause the following error message.

```shell
error[E2G22]: TypeError
 --> pkg_inplace_modify_1/main.k:3:1
  |
6 | pkg |= 2
  | ^ unsupported operand type(s) for |: 'module 'pkg'' and 'int(2)'
  |
```

Possible resolution:

- Check the assignment expression.

1. invalid string expression.

For example:

```python
a = 1
b = "${b = a + 1}" # Invalid string interpolation expression
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> invalid_format_value_fail_0/main.k:2:5
  |
2 | b = "${b = a + 1}"
  |    5 ^ invalid string interpolation expression 'b = a + 1'
  |
```

Possible resolution:

- Check the string expression.

1. invalid loop variable.

For example:

```python
data = {"key1": "value1", "key2": "value2"}
dataLoop = [i for i, j, k in data]  # the number of loop variables can only be 1 or 2
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> dict/invalid_loop_var_fail_0/main.k:2:25
  |
2 | dataLoop = [i for i, j, k in data]  # the number of loop variables can only be 1 or 2
  |                         ^ the number of loop variables is 3, which can only be 1 or 2
  |
```

### 1.2.11 KCLNameError(E2L25)

KCL will report `KCLNameError` when a name error occurs in compiling.

The `ewcode` of `KCLNameError` is `E2L25`.

### 1.2.12 KCLValueError(E2L26)

KCL will report `KCLValueError` will be raised when a value error occurs in compiling.

The `ewcode` of `KCLValueError` is `E2L25`.

### 1.2.13 KCLKeyError(E2L27)

KCL will report `KCLKeyError` will be raised when a key error occurs in compiling.

The `ewcode` of `KCLKeyError` is `E2L25`.

### 1.2.14 UniqueKeyError(E2L28)

KCL will report `UniqueKeyError` when duplicate names appear in the KCL code.

The `ewcode` of `UniqueKeyError` is `E2L28`.

For example:

```python
schema Person:
    name: str = "kcl"
    age: int = 1

schema Person:
    aa: int

x0 = Person{}
x1 = Person{age:101}
```

The KCL program will cause the following error message.

```shell
error[E2L28]: UniqueKeyError
 --> /schema/same_name/main.k:5:8
  |
5 | schema Person:
  |        ^ Unique key error name 'Person'
  |

 --> /schema/same_name/main.k:1:8
  |
1 | schema Person:
  |        ^ The variable 'Person' is declared here
  |
```

Possible resolution:

- Check if the name with error has been used.

### 1.2.15 KCLAttributeComplieError(E2A29)

KCL will report `KCLAttributeComplieError` when KCL has an illegal attribute in the schema.

The `ewcode` of `KCLAttributeComplieError` is `E2A29`.

For example:

```python
# pkg
schema A:
    field_A: str

# main
import pkg as p

a = p.D + 1
```

The KCL program will cause the following error message.

```shell
error[E2G22]: TypeError
 --> /import/module/no_module_attr_fail_0/main.k:4:5
  |
4 | a = p.D + 1
  |     ^ module 'pkg' has no attribute D
  |
```

Possible resolution:

- Check for the existence of the schema attribute when using it.

### 1.2.16 MultiInheritError(E2D32)

KCL will report `MultiInheritError` when multiple inheritance appears in the schema.

The `ewcode` of `MultiInheritError` is `E2D32`.

For example:

```python
schema Person:
    firstName: str
    lastName: str

schema KnowledgeMixin:
    firstName: int
    subject: str

schema Scholar(KnowledgeMixin, Person):
    school: str
```

The KCL program will cause the following error message.

```shell
error[E1001]: InvalidSyntax
 --> /schema/inherit/multi_inherit_fail_1/main.k:9:30
  |
9 | schema Scholar(KnowledgeMixin, Person):
  |                              ^ expected one of [")"] got ,
  |
```

Possible resolution:

- Check the inheritance structure of the program, and multi-inheritance is not supported in KCL.

### 1.2.17 IllegalInheritError(E2D34)

KCL will report `IllegalInheritError` when an illegal inheritance occurs in the schema.

The `ewcode` of `IllegalInheritError` is `E2D34`.

For example:

```python
schema FullnameMixin:
    fullName = "{} {}".format(firstName, lastName)

schema Scholar(FullnameMixin): # mixin inheritance is illegal
    school: str
```

The KCL program will cause the following error message.

```shell
error[E2D34]: IllegalInheritError
 --> /schema/inherit/inherit_mixin_fail/main.k:4:16
  |
4 | schema Scholar(FullnameMixin):
  |                ^ invalid schema inherit object type, expect schema, got 'FullnameMixin'
  |
```

Possible resolution:

- Schema supports single inheritance of schema in KCL.

### 1.2.18 IllegalArgumentComplieError(E2I36)

KCL will report `IllegalArgumentComplieError` when the argument of option in KCL is illegal.

The `ewcode` of `IllegalArgumentComplieError` is `E2I36`.
For example:

```python
a = option("key")

# kcl main.k -D key=value= 
# key=value= is an illegal expression
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
Invalid value for top level arguments
```

Possible resolution:

- Check whether the KCL option arguments are legal.

### 1.2.19 ImmutableCompileError (E3L41)

KCL will report `ImmutableCompileError` when the value of the immutable variable changes.

The `ewcode` of `ImmutableCompileError` is `E3L41`.

For example:

```python
a = 2147483646
a += 1
```

The KCL program will cause the following error message.

```shell
error[E1001]: ImmutableError
 --> augment_assign/main.k:2:1
  |
2 | a += 1
  | ^ Immutable variable 'a' is modified during compiling
  |

 --> augment_assign/main.k:1:1
  |
1 | a = 2147483646
  | ^ The variable 'a' is declared here firstly
  |
note: change the variable name to '_a' to make it mutable
```

Possible resolution:

- Set immutable variables changed to private or remove immutable variables.

## 1.3 KCL Runtime Error (E3xxx)

This section mainly includes KCL errors:

| ewcode | KCL exception                                                          | messages                                            |
| ------ | ---------------------------------------------------------------------- | --------------------------------------------------- |
| E3F06  | [RecursiveLoad](#131-recursiveload-e3f06)                              | Recursively loading module                          |
| E3K04  | [FloatOverflow](#132-floatoverflow-e3k04)                              | Float overflow                                      |
| E3K09  | [IntOverflow](#133-intoverflow-e3k09)                                  | Integer overflow                                    |
| E3N11  | [DeprecatedError](#134-deprecatederror-e3n11)                          | Deprecated error                                    |
| E3A30  | [KCLAttributeRuntimeError](#135-kclattributeruntimeerror-e3a30)        | Attribute error occurs at runtime                   |
| E3G21  | [TypeRuntimeError](#136-typeruntimeerror-e3g21)                        | The type got is inconsistent with the type expected |
| E3B17  | [SchemaCheckFailure](#137-schemacheckfailure-e3b17)                    | Schema check is failed to check condition           |
| E3B19  | [CannotAddMembersRuntimeError](#138-cannotaddmembersruntimeerrore3b19) | Cannot add members to a schema                      |
| E3M38  | [EvaluationError](#139-evaluationerrore3m38)                           | Evaluation failure                                  |
| E3M39  | [InvalidFormatSpec](#1310-invalidformatspec-e3m39)                     | Invalid format specification                        |
| E3M40  | [KCLAssertionError](#1311-kclassertionerror-e3m40)                     | Assertion failure                                   |
| E3M44  | [ImmutableRuntimeError](#1312-immutableruntimeerror-e3m44)             | Immutable variable is modified                      |
| E2D33  | [CycleInheritError](#1313-cycleinheriterror-e2d33)                     | Cycle Inheritance is illegal                        |
| E3M42  | [KCLRecursionError](#1314-kclrecursionerror-e3m42)                     | Recursively reference                               |

### 1.3.1 RecursiveLoad (E3F06)

KCL will report `RecursiveLoad` when a cycle import of external packages occurs in KCL.

The `ewcode` of `RecursiveLoad` is `E2F06`.

For example:

```python
# module.k 
import main # module.k imports main.k

print('module')

# main.k
import module # main.k imports module.k

print('main')
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> /import/recursive_import_fail/main.k:4
  |
2 | import module # main.k imports module.k
  | ^ There is a circular import reference between module main and module
  |
```

Possible resolution:

- Check whether there is a circle import in KCL.

### 1.3.2 FloatOverflow (E3K04)

KCL will report `FloatOverflow` when a floating-point number overflows in KCL.

The `ewcode` of `FloatOverflow` is `E3K04`.

For example:

```python
uplimit = 3.402823466e+39
epsilon = 2.220446049250313e-16
a = uplimit * (1 + epsilon)

# kcl main.k -r -d
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /range_check_float/overflow/number_0/main.k:3:1
  |
3 | a = uplimit * (1 + epsilon)
  |  3.4028234660000003e+39: A 32-bit floating point number overflow
  |
```

Possible resolution:

- Check whether the value of the float is the float range supported by KCL.

### 1.3.3 IntOverflow (E3K09)

KCL will report `IntOverflow` when an integer number overflows in KCL.

The `ewcode` of `IntOverflow` is `E3K09`.

For example:

```python
_a = 9223372036854775807
_a += 1

# kcl test.k -d
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /range_check_int/augment_assign_fail_1/main.k:2:1
  |
2 | _a += 1
  |  9223372036854775808: A 64 bit integer overflow
  |
```

Possible resolution:

- Check whether the value of the integer is the integer range supported by KCL.

### 1.3.4 DeprecatedError (E3N11)

KCL will report `DeprecatedError` when a deprecated variable is used and the strict is True.

The `ewcode` of `DeprecatedError` is `E3N11`.

For example:

```python
schema Person:
    firstName: str = "John"
    lastName: str
    @deprecated(version="1.16", reason="use firstName and lastName instead", strict=True)
    name: str

JohnDoe = Person {
    name: "deprecated" # name is deprecated and strict is True
}
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /range_check_float/overflow/number_0/main.k:7:1
  |
7 | JohnDoe = Person {
  |  name was deprecated since version 1.16, use firstName and lastName instead
  |
```

Possible resolution:

- When strict is set to True, using deprecated code will cause an error and stop KCL.
- You can set the strict to False which will cause a warning insteads of an error.
- Adjust the code without using deprecated code.

### 1.3.5 KCLAttributeRuntimeError (E3A30)

KCL will report `KCLAttributeRuntimeError`, if an error occurs during dynamically accessing schema attributes through variables at runtime.

The `ewcode` of `KCLAttributeRuntimeError` is `E3A30`.

For example:

```python
import math

a = math.err_func(1) # err_func is not found in math
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /import/module/no_module_attr_fail_2/main.k:3:5
  |
3 | a = math.err_func(1) # err_func is not found in math
  |     ^ module 'math' has no attribute err_func
  |
```

Possible resolution:

- Check whether the attributes of schema are correct.

### 1.3.6 TypeRuntimeError (E3G21)

KCL will report `TypeRuntimeError` when an type error occurs in the runtime type check.

The `ewcode` of `TypeRuntimeError` is `E3G21`.

For example:

```python
schema Person:
    name: str = "Alice"

_personA = Person {}
_personA |= {"name" = 123.0} # name: str = "Alice"
personA = _personA
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /fail/fail_4/main.k:2:1
  |
2 |     name: str = "Alice"
  |  expect str, got float
  |
```

Possible resolution:

- Stop the wrong type union or adjust to the type union supported by KCL.

### 1.3.7 SchemaCheckFailure (E3B17)

KCL will report `SchemaCheckFailure` when the schema check conditions are not met.

The `ewcode` of `SchemaCheckFailure` is `E3B17`.

For example:

```python
schema Person:
    lastName: str
    age: int
    check:
        age < 140, "age is too large"

JohnDoe = Person {
    "lastName": "Doe",
    "age": 1000 # the check condition: age < 140
}
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /check_block/check_block_fail_1/main.k:7:11
  |
7 | JohnDoe = Person {
  |           ^ Instance check failed
  |

 --> /check_block/check_block_fail_1/main.k:5:1
  |
5 |         age < 140, "age is too large"
  |  Check failed on the condition
  |
```

Possible resolution:

- Check whether the attributes of schema can satisfy the conditions in check.

### 1.3.8 CannotAddMembersRuntimeError(E3B19)

KCL will report `CannotAddMembersRuntimeError` when members that are not in the schema are used.

The `ewcode` of `CannotAddMembersRuntimeError` is `E3B19`.

For example:

```python
schema Name:
    name: str

schema Person:
    name: Name
    
person = Person {
    name.err_name: "Alice" # err_name is not found in schema Name
}
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> /nest_var/nest_var_fail_1/main.k:8:5
  |
8 |     name.err_name: "Alice" # err_name is not found in schema Name
  |     ^ Cannot add member 'err_name' to schema 'Name'
  |
```

Possible resolution:

- Add a non-existent member to the schema.
- Access members that exist in the schema.

### 1.3.9 EvaluationError(E3M38)

KCL will report `EvaluationError` when an illegal evaluation occurs in KCL.

The `ewcode` of `EvaluationError` is `E3M38`.

For example:

```python
_list1 = [1, 2, 3] # _list1 is a variable, and its type can only be known at runtime
_list2 = None # _list1 is a variable, and its type can only be known at runtime

result2 = _list1 + _list2 # list + NoneType is illegal
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /datatype/list/add_None_fail/main.k:1
  |
4 | result2 = _list1 + _list2 # list + NoneType is illegal
  |  can only concatenate list (not "NoneType") to list
  |
```

Possible resolution:

- Check whether the evaluation of the expression is legal.

### 1.3.10 InvalidFormatSpec (E3M39)

KCL will report `InvalidFormatSpec` when an illegal string format appears in KCL.

The `ewcode` of `InvalidFormatSpec` is `E3M39`.

For example:

```python
a = 1
b = 1
data = "${a: #js}" + " $$ " #  #js is illegal string
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> /datatype/str_interpolation/invalid_format_spec_fail_0/main.k:3
  |
3 | data = "${a: #js}" + " $$ " #  #js is illegal string
  |           ^ #js is a invalid format spec
  |
```

Possible resolution:

- Adjust illegal String to String supported by KCL standards.

### 1.3.11 KCLAssertionError (E3M40)

KCL will report `KCLAssertionError` when assert False occurs in KCL.

The `ewcode` of `KCLAssertionError` is `E3M40`.

For example:

```python
assert False
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /assert/invalid/fail_0/main.k:1
  |
1 | assert False
  | 
  |
```

Possible resolution:

- Check the condition of Assert, and when the Assert condition is False, such an error occurs, removing the Assert statement or changing the condition to True.

### 1.3.12 ImmutableRuntimeError (E3M44)

KCL will report `ImmutableRuntimeError` when the value of the immutable variable changes.

The `ewcode` of `ImmutableRuntimeError` is `E3M44`.

For example:

```python
schema Person:
    final firstName : str
    lastName : str

schema Scholar(Person):
    firstName = "CBA"

scholar = Scholar {
    "firstName": "ABC" # firstName in schema Person is final.
}
```

The KCL program will cause the following error message.

```shell
error[E3M38]: EvaluationError
 --> /final/fail_lazy_init_0/main.k:8:1
  |
8 | scholar = Scholar {
  |  attribute 'lastName' of Scholar is required and can't be None or Undefined
  |
```

Possible resolution:

- Check if the final variables have been assigned or other changes affect the values of the final variables.

### 1.3.13 CycleInheritError (E2D33)

KCL will report `CycleInheritError` when circle inheritance appeared in the schema.

The `ewcode` of `CycleInheritError` is `E2D33`.

For example:

```python
schema Parent(Son):
    parent_field: str

schema Son(GrandSon):
    son_field: str

schema GrandSon(Parent):
    grandson_field: str

parent = Parent {
    parent_field: ""
}
```

The KCL program will cause the following error message.

```shell
error[E2L23]: CompileError
 --> /inherit/cycle_inherit_fail_1/main.k:7:8
  |
7 | schema GrandSon(Parent):
  |        ^ There is a circular reference between schema GrandSon and Parent
  |
```

Possible resolution:

- Check schema inheritance relationship to avoid A inheritance B and B inheritance A at the same time.

### 1.3.14 KCLRecursionError (E3M42)

KCL will report `KCLRecursionError` when a circle reference appears in the program.

The `ewcode` of `KCLRecursionError` is `E3M42`.

For example:

```python
schema Parent(Son):
    parent_field: str
    son: Son = Son {  # Parent has attribute Son
        parent: Parent {
            parent_field: "123"
        }
    }

schema Son:
    son_field: str
    parent: Parent = Parent { # Son has attribute Parent
        son: Son {
            son_field: "123"
        }
    }

parent = Parent {
    parent_field: "",
}
```

The KCL program will cause the following error message.

```shell
thread 'main' has overflowed its stack
fatal runtime error: stack overflow
```

Possible resolution:

- Check the members in the schema to avoid the problem of circle references.

## 1.4 KCL Compile Warning (W2xxx)

This section mainly includes KCL warnings:

| ewcode | KCL exception                                     | messages           |
| ------ | ------------------------------------------------- | ------------------ |
| W2K04  | [FloatUnderflow](#141-floatunderflow-w2k04)       | Float underflow    |
| W2P10  | [InvalidDocstring](#142-invaliddocstring-w2p10)   | Invalid docstring  |
| W2N12  | [DeprecatedWarning](#143-deprecatedwarning-w2n12) | Deprecated warning |

### 1.4.1 FloatUnderflow (W2K04)

KCL will report `FloatUnderflow`  when a floating-point number underflows in KCL.

The `ewcode` of `FloatUnderflow` is `W2K08`.

Possible resolution:

- Check whether the value of the float number is in the range supported by KCL.

### 1.4.2 InvalidDocstring (W2P10)

KCL will report `InvalidDocstring` when a string is illegal in KCL doc.

The `ewcode` of `InvalidDocstring` is `W2P10`.

Possible resolution:

- Please write doc according to KCL standards.

### 1.4.3 DeprecatedWarning (W2N12)

KCL will report `DeprecatedWarning` when a deprecated variable is used and the strict is False.

The `ewcode` of `DeprecatedWarning` is `W2N12`.

Possible resolution:

- Try not to use deprecated code. If the strict is True, KCL will output the error and stop running.