<div align="center">

<image src="https://github.com/user-attachments/assets/8f5a17f3-bf61-4d33-8ddd-dbec6d2c5a57" height="100"/>

---

基于 JSON 二次改编的新一代轻量级数据交互格式

</div>

## 简介

SRON 是一种对 JSON 格式的扩展，旨在提高其在配置文件和数据交换中的灵活性和可读性。它在保持与标准 JSON 兼容性的同时，引入了类型推断、显式类型覆盖、多行字符串支持等特性，使得配置文件的编写更加方便和强大。

## 特性

> [!Important]
>
> 目前 SRON 的规范仍在不断完善，部分规范内容可能不会在正式版中上线，且目前的规范可能存在一定包括但不限于逻辑问题、语法问题、规则冲突问题等，请谨慎使用。
>
> 当前版本号: v0.1-aplha

SRON 旨在解决 JSON 在某些场景下的局限性，例如：

*   **类型推断：** 自动推断常见数据类型，无需显式指定。
*   **显式类型覆盖：** 允许通过 `__type__` 键显式指定数据类型。
*   **多行字符串：** 支持使用 `>` 和 `|` 符号编写多行字符串。
*   **环境变量引用：** 支持在字符串中引用环境变量。

与标准 JSON 的对比：

| 特性            | JSON                               | SRON                                                            |
| --------------- | ---------------------------------- | ---------------------------------------------------------------- |
| 类型推断          | 不支持                             | 支持自动推断数字、布尔值等类型                                     |
| 显式类型覆盖      | 不支持                             | 通过 `__type__` 键显式指定类型                                       |
| 多行字符串        | 不支持                             | 支持使用 `>` 和 `|` 符号                                                    |
| 环境变量引用      | 不支持                             | 支持在字符串中使用 `${ENV_VAR}` 引用环境变量                               |
| 注释             | 不支持                             |  (可以通过预处理移除注释后再解析)                                      |
| 可读性            | 相对简单，但缺乏灵活性               | 更灵活，可以根据需求选择不同的书写方式，提高可读性                           |
| 适用场景          | 简单的数据交换                     | 配置文件、复杂的API数据传输等                                       |

## 规范

SRON 的标准文件扩展名为 ```.Srd``` （SRInternet-Object Notation Data），其在标准 JSON 的基础上，增加了以下规范：

### 1. 类型推断

SRON 会自动推断以下类型：

*   数字：如果字符串可以转换为数字，则视为数字类型。
*   布尔值：`true` 和 `false` 字符串视为布尔值。
*   Null: `null` 字符串视为 Null 值。

### 2. 显式类型覆盖

可以使用 `__type__` 键来显式指定数据类型。 例如：

```json
{
  "version": "1",
  "enabled": "true",
  "port": "8080"
}
```

会被解析为字符串类型的 "1", "true" 和 "8080"。  如果希望将 "true" 解析为布尔值，则可以使用 `__type__` 键：

```json
{
  "version": "1",
  "enabled": {
    "__type__": "boolean",
    "value": "true"
  },
  "port": "8080"
}
```

常用的 `__type__` 值：

*   `string`：字符串
*   `number`：数字
*   `boolean`：布尔值
*   `null`：空值

### 3. 多行字符串

可以使用 `>` 和 `|` 符号编写多行字符串。

*   `>`：折叠换行符，将多个物理行合并为一个逻辑行。
*   `|`：保留换行符，保留多个物理行的换行符。

示例：

```json
{
  "description": ">
    This is a long description
    that spans multiple lines.
    The newlines will be folded into spaces.
  ",
  "log_message": "|
    This is a log message
    that spans multiple lines.
    The newlines will be preserved.
  "
}
```

### 4. 环境变量引用

可以在字符串中使用 `${ENV_VAR}` 引用环境变量。 例如：

```json
{
  "log_path": "/var/log/${APP_NAME}.log"
}
```

### SRON 格式模板

```json
{
  "name": "My Application",
  "version": {
    "__type__": "number",
    "value": "1.2.3"
  },
  "enabled": {
    "__type__": "boolean",
    "value": "true"
  },
  "port": "8080",
  "description": ">
    This is a long description
    that spans multiple lines.
    The newlines will be folded into spaces.
  ",
  "log_path": "/var/log/${APP_NAME}.log",
  "admins": [
    "user1@example.com",
    "user2@example.com"
  ],
  "database": {
    "host": "localhost",
    "port": {
      "__type__": "number",
      "value": "5432"
    },
    "username": "dbuser",
    "password": "dbpassword"
  },
  "null_value": null,
  "multiline_string": "|
    This is a multiline string
    with preserved newlines.
  "
}
```

## 使用方法

我们正在编写 SRON 在 Python 上的标准库和在 .NET Framework 上的 NuGet 包，预计在几个月之内上线，稍后请见 SRON_Python 和 SRON.NET 仓库，敬请期待。

> [!Note]
>
> SRON 基于标准 JSON 格式二次改编，因此具有一定的 JSON 兼容性。在我们的标准库和包上线之前，你可以将 SRON 格式当做标准 JSON 格式进行解析，但无法体验到 SRON 的特性功能。

## 贡献

欢迎提交 Pull Request 来改进 SRON！

## 许可证

我们使用 [MIT Licence](./LICENSE) 来保护我们的规范。
