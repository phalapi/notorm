# PhalApi-NotORM（数据库底层包）

基于 [NotORM](https://www.notorm.com/) 类库的优化版本，专门用于 [PhalApi 2.x 开源接口框架](https://www.phalapi.net)。

## 版本与运行环境

| 项目 | 说明 |
|---|---|
| 当前版本 | **v8.0.0** |
| PHP 要求 | **>= 8.0**（已实测 8.0 与 8.4） |
| PHP 7.x | 不支持 |

> **版本号说明**：`v8.0.0` 中的 **8 表示“支持 PHP 8”**，不是 PhalAPI 的第 8 代版本。

## 安装

通常不需要单独安装，`phalapi/kernal` 已依赖本包，随框架一并引入：

```bash
composer require phalapi/notorm:^8.0
```

## v8.0.0 主要变更

本版本只做 PHP 8 兼容性修复，**没有功能变更，也没有 API 变化**：

+ `NotORM_Row`、`NotORM_Result`、`NotORM_MultiResult` 对 `ArrayAccess`、`Iterator`、`Countable`、`JsonSerializable` 的实现补 `#[\ReturnTypeWillChange]`，消除 PHP 8.1 起的内部接口返回类型弃用；
+ 构造函数与 `__set()` 的隐式可空参数改为显式 `?Type`，消除 PHP 8.4 弃用；
+ `NotORM_Result::__call()` 中 `trigger_error(..., E_USER_ERROR)` 改为抛出 `BadMethodCallException`（前者在 PHP 8.4 已弃用）。

### 行为变化

调用不存在的方法时，原来触发 `E_USER_ERROR` 终止脚本，现在抛出 `BadMethodCallException`。两者都会中断流程，但异常可以被 `try/catch` 捕获，也能被统一异常处理接管。

## 相关仓库

+ [phalapi/kernal](https://github.com/phalapi/kernal) 框架内核
+ [phalapi/phalapi](https://github.com/phalapi/phalapi) 完整项目
+ [升级到 v8.0.0（PHP 8 支持）](http://docs.phalapi.net/#/v2.0/php8-upgrade) 完整升级指南
