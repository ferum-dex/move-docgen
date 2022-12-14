# Move编码规范 （Move Coding Conventions）

This section lays out some basic coding conventions for Move that the Move team has found helpful. These are only recommendations, and you should feel free to use other formatting guidelines and conventions if you have a preference for them.

本部分列出了 Move 团队认为有用的一些基本编码规范。这些只是建议，如果您有偏好，可以随意使用其他格式的指南和规范。

## 命名（Naming）

- **Module names**: should be lower snake case, e.g., `fixed_point32`, `vector`
- **Type names**: should be camel case if they are not a native type, e.g., `Coin`, `RoleId`
- **Function names**: should be lower snake case, e.g., `destroy_empty`
- **Constant names**: should be upper snake case, e.g., `REQUIRES_CAPABILITY`
- Generic types should be descriptive, or anti-descriptive where appropriate, e.g., `T` or `Element` for the Vector generic type parameter. Most of the time the "main" type in a module should be the same name as the module e.g., `option::Option`, `fixed_point32::FixedPoint32`.
- **Module file names**: should be the same as the module name e.g., `Option.move`
- **Script file names**: should be lower snake case and should match the name of the “main” function in the script.
- **Mixed file names**: If the file contains multiple modules and/or scripts, the file name should be lower_snake_case, where the name does not match any particular module/script inside

<br />

- **模块命名**: 应该使用小写的蛇形命名法, 例如: `fixed_point32`, `vector`
- **类型命名**: 如果不是原生数据类型, 应该使用驼峰命名法, 例如: `Coin`, `RoleId`
- **函数命名**: 应该使用小写的蛇形命名法, 例如: `destroy_empty`
- **常量命名**: 应该使用大写的蛇形命名法, 例如: `REQUIRES_CAPABILITY`
- 泛型类型命名应该具备描述性, 当然在适当的情况下也可以不具备非描述性, 例如: Vector 泛型类型的参数可以是 `T` 或 `Element`。 大多数情况下，模块中的"主要"类型命名应该与模块名相同, 例如: `option::Option`, `fixed_point32::FixedPoint32`.
- **模块文件命名**: 应该与模块名相同, 例如: `Option.move`
- **脚本文件命名**: 应该使用小写的蛇形命名法, 并且应该与脚本中的"主要"函数名匹配。
- **混合文件命名**: 如果文件包含多个模块和/或脚本, 文件命名应该使用小写的蛇形命名法, 并且不需要与内部的任何特定模块/脚本名匹配。

## 导入（Imports）

- All module `use` statements should be at the top of the module.
- Functions should be imported and used fully qualified from the module in which they are declared, and not imported at the top level.
- Types should be imported at the top-level. Where there are name clashes, `as` should be used to rename the type locally as appropriate.

<br />

- 所有模块的`use`语句都应该位于模块的顶部。
- 函数应该从声明它们的模块中完全限定地导入和使用, 而不是在顶部导入。
- 类型应该在顶部导入。如果有名称冲突, 应使用`as`在本地重命名类型。

For example, if there is a module:

例如, 如果有一个模块：

```move=
module 0x1::foo {
    struct Foo { }
    const CONST_FOO: u64 = 0;
    public fun do_foo(): Foo { Foo{} }
    ...
}
```
this would be imported and used as:

此时将被导入并使用：

```move=
module 0x1::bar {
    use 0x1::foo::{Self, Foo};

    public fun do_bar(x: u64): Foo {
        if (x == 10) {
            foo::do_foo()
        } else {
            abort 0
        }
    }
    ...
}
```
And, if there is a local name-clash when importing two modules:

并且, 如果在导入两个模块存在本地名称冲突时：

```move=
module other_foo {
    struct Foo {}
    ...
}

module 0x1::importer {
    use 0x1::other_foo::Foo as OtherFoo;
    use 0x1::foo::Foo;
....
}
```

## 注释（Comments）

- Each module, struct, and public function declaration should be commented
- Move has doc comments `///`, regular single-line comments `//`, block comments `/* */`, and block doc comments `/** */`

<br />

- 每个模块、结构体和公共函数声明都应该有对应的注释。
- Move有文档注释 `///`, 常规单行注释 `//`, 块注释 `/* */`, 和块文档注释 `/** */`。

## 格式化（Formatting）

The Move team plans to write an autoformatter to enforce formatting conventions. However, in the meantime:

Move团队计划编写一个自动格式化程序来执行格式化规范。然而, 在此期间:

- Four space indentation should be used except for `script` and `address` blocks whose contents should not be indented
- Lines should be broken if they are longer than 100 characters
- Structs and constants should be declared before all functions in a module

<br />

- 除`script`和`address`块的内容外, 应使用四个空格的缩进。
- 如果超过100个字符, 应该换行。
- 结构体和常量应该在模块中的所有函数之前声明。
