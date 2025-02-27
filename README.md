<div align="center">

<image src="https://github.com/user-attachments/assets/8f5a17f3-bf61-4d33-8ddd-dbec6d2c5a57" height="100"/>

---
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-0.1_alpha-yellow.svg)](VERSION)

新一代统一和简化配置管理的通用配置文件格式标准，易于阅读、编写、验证和扩展的配置语言。

</div>

## 简介

**SRON (SRInternet-Object Notation)** 是一种统一和简化配置管理的通用配置文件格式，旨在提供易于阅读、编写、验证和扩展的配置语言，适用于各种应用程序和服务。

> [!Important]
>
> 当前标准正在不断完善中，可能存在某些逻辑问题或其他瑕疵，使用时请格外注意。
>
> 当前版本：0.1-alpha

## 基本特性

*   **简洁性：** 降低语法复杂性，提供更易于理解和编写的配置语言。
*   **可读性：** 通过缩进、注释和多行字符串等特性提高配置文件的可读性。
*   **灵活性：** 允许开发者根据需要选择是否使用类型指定和命名空间。

## 为什么选择 SRON

> 此部分内容在正式版发布之前会不断更新，部分特性可能不会在正式版中发布

### 与 JSON

| 特性             | JSON                     | SRON                                      |
| ---------------- | ------------------------ | ---------------------------------------- |
| 注释             | 不支持                   | 支持单行 (`//`, `#`) 和多行 (`/* ... */`) 注释 |
| 多行字符串         | 需要转义                 | 支持三重引号 (`"""` 或 `'''`)            |
| 类型指定           | 不支持显式类型指定     | 支持可选的类型指定 (`key : type = value`)   |
| 命名空间           | 不支持                   | 支持命名空间 (`namespace.key = value`)       |
| 语法             | 简洁，但可读性稍差     | 简洁且可读性相对较高                            |
| 适用场景         | API 数据交换、简单配置 | 配置文件、需要类型信息的配置（不建议用于 API ）         |

### 与 YAML

| 特性             | YAML                       | SRON                                     |
| ---------------- | -------------------------- | --------------------------------------- |
| 缩进             | 强制                     | 强制                                    |
| 多行字符串         | 多种表示方式，相对复杂   | 使用三重引号 (`"""` 或 `'''`) |
| 类型指定           | 主要依赖隐式类型推断     | 支持可选的类型指定 (`key : type = value`)  |
| 锚点和别名         | 支持                     | 不支持                                  |
| 语法             | 灵活，相对复杂     | 简洁，相对更易于入门                            |
| 适用场景         | 复杂配置、需要人工编辑   | 配置文件                               |

### 与 TOML

| 特性             | TOML                     | SRON                                      |
| ---------------- | ------------------------ | ---------------------------------------- |
| 注释             | 非官方支持               | 支持单行 (`//`, `#`) 和多行 (`/* ... */`) 注释 |
| 多行字符串         | 不支持原生多行字符串     | 支持三重引号 (`"""` 或 `'''`)            |
| 类型指定           | 有限的内置类型           | 支持可选的类型指定 (`key : type = value`)   |
| 命名空间           | 使用 section             | 使用命名空间 (`namespace.key = value`)       |
| 语法             | 简洁         | 简洁                             |
| 适用场景         | 简单配置                 | 配置文件、需要更丰富特性的配置           |

## 规范

> 此部分内容在正式版发布之前会不断更新，部分标准可能在正式版中会有所变更

SRON 的标准文件扩展名为 ```.Srd``` （SRInternet-Object Notation Data），该类型文件具有以下结构特征：

### 1. 基本结构

SRON 文件由配置项、命名空间和列表组成。 文件内容以 UTF-8 编码。

*   **配置项 (Configuration Item)：** 使用 `key = value` 的形式表示配置项，其中 `key` 是配置的名称，`value` 是配置的值。
*   **命名空间 (Namespace)：** 用于组织和隔离配置项，避免命名冲突。 使用 `namespace.key = value` 的形式表示。
*   **列表 (List)：** 用于表示一组相同类型的值。 使用 `[value1, value2, ...]` 的形式表示。

### 2. 语法规范

*   **空白字符 (Whitespace)：** 忽略行首和行尾的空白字符。 使用缩进表示层级关系。
*   **注释 (Comments)：**
    *   单行注释：使用 `//` 或 `#` 开头，直到行尾。
    *   多行注释：使用 `/* ... */` 包裹。
*   **标识符 (Identifiers)：**
    *   配置项的名称（`key`）必须以字母或下划线开头，可以包含字母、数字和下划线。
    *   命名空间的名称也必须遵循相同的规则。
*   **字符串 (Strings)：**
    *   可以使用双引号 (`"`) 或单引号 (`'`) 包裹。
    *   支持转义字符 (例如 `\
`, `\	`, `\"`, `\\`)。
    *   支持三重引号 (`"""` 或 `'''`) 表示多行字符串，保留换行符和缩进。
*   **数字 (Numbers)：**
    *   支持整数 (例如 `123`, `-456`) 和浮点数 (例如 `3.14`, `-0.5`)。
    *   支持科学计数法 (例如 `1e6`, `1.23e-4`)。
*   **布尔值 (Booleans)：**
    *   使用 `true` 或 `false` 表示。
*   **空值 (Null)：**
    *   使用 `null` 表示。
*   **类型指定 (Type Specification)：**
    *   可选的类型指定，用于显式指定配置项的类型。
    *   使用 `key : type = value` 的形式表示，其中 `type` 是类型名称。
    *   如果省略类型指定，则默认为字符串类型。
*   **列表 (Lists)：**
    *   使用方括号 `[]` 包裹，元素之间使用逗号 `,` 分隔。
    *   例如：`allowed_origins = ["https://example.com", "https://localhost:3000"]`
    *   如果列表元素类型相同，可以省略类型指定，例如 `ports : integer[] = [8080, 8081, 8082]`。
*   **命名空间 (Namespaces)：**
    *   用于组织和隔离配置项。
    *   使用 `namespace.key = value` 的形式表示，其中 `namespace` 是命名空间的名称。
    *   可以有多级命名空间，例如 `parent.child.key = value`。
    *   全局命名空间：直接使用 `key = value` 表示，不需要指定命名空间。

### 3. 数据类型

SRON 支持以下基本数据类型：

*   `string`：字符串。
*   `integer`：整数。
*   `float`：浮点数。
*   `boolean`：布尔值。
*   `null`：空值。
*   `array`：列表，元素类型相同。

SRON 还支持扩展更多数据类型（需要开发者自行解析[^1]）：

*   `date`：日期。
*   `time`：时间。
*   `datetime`：日期时间。
*   `markdown`：Markdown 格式的文本。
*   自定义类型：由插件定义。

## 示例

```python
// Global settings
name = "A SRON based Application"
version = "1.0.0"
debug = true

// Server configuration
server.host = "localhost"
server.port :integer = 8080
server.timeout = 30  // seconds

// Database configuration
database.url = "jdbc:mysql://localhost:3306/mydb"
database.max_connections :integer = 100

// Multi-line SQL query
database.query = """
  SELECT *
  FROM users
  WHERE status = 'active';
"""

// List of allowed origins
allowed_origins = ["https://www.example.com", "https://localhost:3000"]

// Plugin configuration
plugin.markdown.renderer = "markdown-it"
```


## 使用方法

我们正在编写 SRON 在 Python 上的标准库和在 .NET Framework 上的 NuGet 包，预计在几个月之内上线，稍后请见 SRON_Python 和 SRON.NET 仓库，敬请期待。

## 贡献

欢迎提交 Pull Request 来改进 SRON！

## 许可证

我们使用 [MIT Licence](./LICENSE) 来保护我们的规范。

[^1]:SRON 支持的数据类型 是指原生解析器当前能够解析的数据类型，当开发者在代码中引用这些数据时，将以指定的相应数据格式返回。扩展的类型，解析器则无法以指定的相应数据格式返回，默认均返回字符串 (string)。开发者可以通过指定方法获取数据的指定类型，自行进行解析。
