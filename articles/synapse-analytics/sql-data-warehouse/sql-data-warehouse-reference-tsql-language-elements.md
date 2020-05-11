---
title: T-SQL 语言元素
description: Synapse SQL 池支持的 T-SQL 语句的相关文档的链接。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 06/13/2018
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 98257fdac01c7558e335b0afcd611c2d90b936dd
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198794"
---
# <a name="t-sql-language-elements-supported-in-synapse-sql-pool"></a>Synapse SQL 池支持的 T-SQL 语言元素

Synapse SQL 池支持的 T-SQL 语言元素的相关文档的链接。

## <a name="core-elements"></a>核心元素

* [语法约定](https://docs.microsoft.com/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [对象命名规则](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [保留的关键字](https://docs.microsoft.com/sql/t-sql/language-elements/reserved-keywords-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [排序规则](https://docs.microsoft.com/sql/t-sql/statements/collations?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [注释](https://docs.microsoft.com/sql/t-sql/language-elements/comment-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [常量](https://docs.microsoft.com/sql/t-sql/data-types/constants-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [数据类型](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [EXECUTE](https://docs.microsoft.com/sql/t-sql/language-elements/execute-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [表达式](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [KILL](https://docs.microsoft.com/sql/t-sql/language-elements/kill-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [IDENTITY 属性解决方法](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql-identity-property?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [PRINT](https://docs.microsoft.com/sql/t-sql/language-elements/print-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [USE](https://docs.microsoft.com/sql/t-sql/language-elements/use-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="batches-control-of-flow-and-variables"></a>批、流控制和变量

* [BEGIN...END](https://docs.microsoft.com/sql/t-sql/language-elements/begin-end-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [BREAK](https://docs.microsoft.com/sql/t-sql/language-elements/break-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DECLARE @local_variable](https://docs.microsoft.com/sql/t-sql/language-elements/declare-local-variable-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [IF...ELSE](https://docs.microsoft.com/sql/t-sql/language-elements/if-else-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [RAISERROR](https://docs.microsoft.com/sql/t-sql/language-elements/raiserror-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET@local_variable](https://docs.microsoft.com/sql/t-sql/language-elements/set-local-variable-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [THROW](https://docs.microsoft.com/sql/t-sql/language-elements/throw-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [TRY...CATCH](https://docs.microsoft.com/sql/t-sql/language-elements/try-catch-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [WHILE](https://docs.microsoft.com/sql/t-sql/language-elements/while-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="operators"></a>运算符

* [+（加）](https://docs.microsoft.com/sql/t-sql/language-elements/add-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [+（字符串串联）](https://docs.microsoft.com/sql/t-sql/language-elements/string-concatenation-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [-（负）](https://docs.microsoft.com/sql/t-sql/language-elements/unary-operators-negative?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [-（减）](https://docs.microsoft.com/sql/t-sql/language-elements/subtract-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [*（乘）](https://docs.microsoft.com/sql/t-sql/language-elements/multiply-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [/（除）](https://docs.microsoft.com/sql/t-sql/language-elements/divide-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [取模](https://docs.microsoft.com/sql/t-sql/language-elements/modulo-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="wildcard-characters-to-match"></a>要匹配的通配符

* [=（等于）](https://docs.microsoft.com/sql/t-sql/language-elements/equals-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [>（大于）](https://docs.microsoft.com/sql/t-sql/language-elements/greater-than-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [<（小于）](https://docs.microsoft.com/sql/t-sql/language-elements/less-than-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [>=（大于或等于）](https://docs.microsoft.com/sql/t-sql/language-elements/greater-than-or-equal-to-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [<=（小于或等于）](https://docs.microsoft.com/sql/t-sql/language-elements/less-than-or-equal-to-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [<>（不等于）](https://docs.microsoft.com/sql/t-sql/language-elements/not-equal-to-transact-sql-traditional?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [!=（不等于）](https://docs.microsoft.com/sql/t-sql/language-elements/not-equal-to-transact-sql-exclamation?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [AND](https://docs.microsoft.com/sql/t-sql/language-elements/and-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [BETWEEN](https://docs.microsoft.com/sql/t-sql/language-elements/between-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [EXISTS](https://docs.microsoft.com/sql/t-sql/language-elements/exists-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [IN](https://docs.microsoft.com/sql/t-sql/language-elements/in-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [IS [NOT] NULL](https://docs.microsoft.com/sql/t-sql/queries/is-null-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [NOT](https://docs.microsoft.com/sql/t-sql/language-elements/not-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [OR](https://docs.microsoft.com/sql/t-sql/language-elements/or-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

### <a name="bitwise-operators"></a>位运算符

* [&（位与）](https://docs.microsoft.com/sql/t-sql/language-elements/bitwise-and-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [|（位或）](https://docs.microsoft.com/sql/t-sql/language-elements/bitwise-or-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [^（位异或）](https://docs.microsoft.com/sql/t-sql/language-elements/bitwise-exclusive-or-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [~（位非）](https://docs.microsoft.com/sql/t-sql/language-elements/bitwise-not-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [^=（位异或等于）](https://docs.microsoft.com/sql/t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [|=（位或等于）](https://docs.microsoft.com/sql/t-sql/language-elements/bitwise-or-equals-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [&=（位与等于）](https://docs.microsoft.com/sql/t-sql/language-elements/bitwise-and-equals-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="functions"></a>函数

* [@@DATEFIRST](https://docs.microsoft.com/sql/t-sql/functions/datefirst-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [@@ERROR]https://docs.microsoft.com/sql/t-sql/functions/error-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [@@LANGUAGE](https://docs.microsoft.com/sql/t-sql/functions/language-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [@@SPID](https://docs.microsoft.com/sql/t-sql/functions/spid-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [@@TRANCOUNT](https://docs.microsoft.com/sql/t-sql/functions/trancount-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [@@VERSION](https://docs.microsoft.com/sql/t-sql/functions/version-transact-sql-configuration-functions?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ABS](https://docs.microsoft.com/sql/t-sql/functions/abs-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ACOS](https://docs.microsoft.com/sql/t-sql/functions/acos-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ASCII]https://docs.microsoft.com/sql/t-sql/functions/ascii-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ASIN](https://docs.microsoft.com/sql/t-sql/functions/asin-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ATAN](https://docs.microsoft.com/sql/t-sql/functions/atan-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ATN2](https://docs.microsoft.com/sql/t-sql/functions/atn2-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [BINARY_CHECKSUM]https://docs.microsoft.com/sql/t-sql/functions/binary-checksum-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CASE](https://docs.microsoft.com/sql/t-sql/language-elements/case-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CAST 和 CONVERT](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CEILING](https://docs.microsoft.com/sql/t-sql/functions/ceiling-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CHAR](https://docs.microsoft.com/sql/t-sql/functions/char-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CHARINDEX](https://docs.microsoft.com/sql/t-sql/functions/charindex-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CHECKSUM](https://docs.microsoft.com/sql/t-sql/functions/checksum-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [COALESCE](https://docs.microsoft.com/sql/t-sql/language-elements/coalesce-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [COL_NAME](https://docs.microsoft.com/sql/t-sql/functions/col-name-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [COLLATIONPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/collation-functions-collationproperty-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CONCAT](https://docs.microsoft.com/sql/t-sql/functions/concat-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [COS](https://docs.microsoft.com/sql/t-sql/functions/cos-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [COT](https://docs.microsoft.com/sql/t-sql/functions/cot-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [COUNT](https://docs.microsoft.com/sql/t-sql/functions/count-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [COUNT_BIG](https://docs.microsoft.com/sql/t-sql/functions/count-big-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CUME_DIST](https://docs.microsoft.com/sql/t-sql/functions/cume-dist-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CURRENT_TIMESTAMP](https://docs.microsoft.com/sql/t-sql/functions/current-timestamp-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [CURRENT_USER](https://docs.microsoft.com/sql/t-sql/functions/current-user-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATABASEPROPERTYEX](https://docs.microsoft.com/sql/t-sql/functions/databasepropertyex-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATALENGTH](https://docs.microsoft.com/sql/t-sql/functions/datalength-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATEADD](https://docs.microsoft.com/sql/t-sql/functions/dateadd-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATEDIFF](https://docs.microsoft.com/sql/t-sql/functions/datediff-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATEFROMPARTS](https://docs.microsoft.com/sql/t-sql/functions/datefromparts-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATENAME](https://docs.microsoft.com/sql/t-sql/functions/datename-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATEPART](https://docs.microsoft.com/sql/t-sql/functions/datepart-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATETIME2FROMPARTS](https://docs.microsoft.com/sql/t-sql/functions/datetime2fromparts-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATETIMEFROMPARTS](https://docs.microsoft.com/sql/t-sql/functions/datetimefromparts-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DATETIMEOFFSETFROMPARTS](https://docs.microsoft.com/sql/t-sql/functions/datetimeoffsetfromparts-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DAY](https://docs.microsoft.com/sql/t-sql/functions/day-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DB_ID](https://docs.microsoft.com/sql/t-sql/functions/db-id-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DB_NAME](https://docs.microsoft.com/sql/t-sql/functions/db-name-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DEGREES](https://docs.microsoft.com/sql/t-sql/functions/degrees-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DENSE_RANK](https://docs.microsoft.com/sql/t-sql/functions/dense-rank-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [DIFFERENCE](https://docs.microsoft.com/sql/t-sql/functions/difference-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [EOMONTH](https://docs.microsoft.com/sql/t-sql/functions/eomonth-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ERROR_MESSAGE](https://docs.microsoft.com/sql/t-sql/functions/error-message-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ERROR_NUMBER](https://docs.microsoft.com/sql/t-sql/functions/error-number-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ERROR_PROCEDURE](https://docs.microsoft.com/sql/t-sql/functions/error-procedure-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ERROR_SEVERITY](https://docs.microsoft.com/sql/t-sql/functions/error-severity-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ERROR_STATE](https://docs.microsoft.com/sql/t-sql/functions/error-state-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [EXP](https://docs.microsoft.com/sql/t-sql/functions/exp-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [FIRST_VALUE](https://docs.microsoft.com/sql/t-sql/functions/first-value-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [FLOOR](https://docs.microsoft.com/sql/t-sql/functions/floor-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [GETUTCDATE](https://docs.microsoft.com/sql/t-sql/functions/getutcdate-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [HAS_DBACCESS](https://docs.microsoft.com/sql/t-sql/functions/has-dbaccess-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [HASHBYTES](https://docs.microsoft.com/sql/t-sql/functions/hashbytes-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [INDEXPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/indexproperty-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ISDATE](https://docs.microsoft.com/sql/t-sql/functions/isdate-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ISNULL](https://docs.microsoft.com/sql/t-sql/functions/isnull-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ISNUMERIC](https://docs.microsoft.com/sql/t-sql/functions/isnumeric-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LAG](https://docs.microsoft.com/sql/t-sql/functions/lag-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LAST_VALUE](https://docs.microsoft.com/sql/t-sql/functions/last-value-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LEAD](https://docs.microsoft.com/sql/t-sql/functions/lead-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LEFT](https://docs.microsoft.com/sql/t-sql/functions/left-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LEN](https://docs.microsoft.com/sql/t-sql/functions/len-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LOG](https://docs.microsoft.com/sql/t-sql/functions/log-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LOG10](https://docs.microsoft.com/sql/t-sql/functions/log10-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LOWER](https://docs.microsoft.com/sql/t-sql/functions/lower-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [LTRIM](https://docs.microsoft.com/sql/t-sql/functions/ltrim-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [MAX](https://docs.microsoft.com/sql/t-sql/functions/max-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [MIN](https://docs.microsoft.com/sql/t-sql/functions/min-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [MONTH](https://docs.microsoft.com/sql/t-sql/functions/month-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [NCHAR](https://docs.microsoft.com/sql/t-sql/functions/nchar-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [NTILE](https://docs.microsoft.com/sql/t-sql/functions/ntile-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [NULLIF](https://docs.microsoft.com/sql/t-sql/language-elements/nullif-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [OBJECT_ID](https://docs.microsoft.com/sql/t-sql/functions/object-id-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [OBJECT_NAME](https://docs.microsoft.com/sql/t-sql/functions/object-name-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [OBJECTPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/objectproperty-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [OIBJECTPROPERTYEX](https://docs.microsoft.com/sql/t-sql/functions/objectpropertyex-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ODBCS 标量函数](https://docs.microsoft.com/sql/t-sql/functions/odbc-scalar-functions-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [OVER 子句](https://docs.microsoft.com/sql/t-sql/queries/select-over-clause-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [PARSENAME](https://docs.microsoft.com/sql/t-sql/functions/parsename-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [PATINDEX](https://docs.microsoft.com/sql/t-sql/functions/patindex-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [PERCENTILE_CONT](https://docs.microsoft.com/sql/t-sql/functions/percentile-cont-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [PERCENTILE_DISC](https://docs.microsoft.com/sql/t-sql/functions/percentile-disc-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [PERCENT_RANK](https://docs.microsoft.com/sql/t-sql/functions/percent-rank-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [PI](https://docs.microsoft.com/sql/t-sql/functions/pi-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [POWER](https://docs.microsoft.com/sql/t-sql/functions/power-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [QUOTENAME](https://docs.microsoft.com/sql/t-sql/functions/quotename-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [RADIANS](https://docs.microsoft.com/sql/t-sql/functions/radians-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [RAND](https://docs.microsoft.com/sql/t-sql/functions/rand-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [RANK](https://docs.microsoft.com/sql/t-sql/functions/rank-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [REPLACE](https://docs.microsoft.com/sql/t-sql/functions/replace-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [REPLICATE](https://docs.microsoft.com/sql/t-sql/functions/replicate-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [REVERSE](https://docs.microsoft.com/sql/t-sql/functions/reverse-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [RIGHT](https://docs.microsoft.com/sql/t-sql/functions/right-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ROUND](https://docs.microsoft.com/sql/t-sql/functions/round-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [ROW_NUMBER](https://docs.microsoft.com/sql/t-sql/functions/row-number-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [RTRIM](https://docs.microsoft.com/sql/t-sql/functions/rtrim-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SCHEMA_ID](https://docs.microsoft.com/sql/t-sql/functions/schema-id-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SCHEMA_NAME](https://docs.microsoft.com/sql/t-sql/functions/schema-name-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SESSION_USER](https://docs.microsoft.com/sql/t-sql/functions/session-user-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SIGN](https://docs.microsoft.com/sql/t-sql/functions/sign-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SIN](https://docs.microsoft.com/sql/t-sql/functions/sin-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SMALLDATETIMEFROMPARTS](https://docs.microsoft.com/sql/t-sql/functions/smalldatetimefromparts-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SOUNDEX](https://docs.microsoft.com/sql/t-sql/functions/soundex-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SPACE](https://docs.microsoft.com/sql/t-sql/functions/space-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SQL_VARIANT_PROPERTY](https://docs.microsoft.com/sql/t-sql/functions/sql-variant-property-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SQRT](https://docs.microsoft.com/sql/t-sql/functions/sqrt-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SQUARE](https://docs.microsoft.com/sql/t-sql/functions/square-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [STATS_DATE](https://docs.microsoft.com/sql/t-sql/functions/stats-date-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [STDEV](https://docs.microsoft.com/sql/t-sql/functions/stdev-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [STDEVP](https://docs.microsoft.com/sql/t-sql/functions/stdevp-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [STR](https://docs.microsoft.com/sql/t-sql/functions/str-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [STUFF](https://docs.microsoft.com/sql/t-sql/functions/stuff-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SUBSTRING](https://docs.microsoft.com/sql/t-sql/functions/substring-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SUM](https://docs.microsoft.com/sql/t-sql/functions/sum-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SUSER_SNAME](https://docs.microsoft.com/sql/t-sql/functions/suser-sname-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SWITCHOFFSET](https://docs.microsoft.com/sql/t-sql/functions/switchoffset-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SYSDATETIME](https://docs.microsoft.com/sql/t-sql/functions/sysdatetime-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SYSDATETIMEOFFSET](https://docs.microsoft.com/sql/t-sql/functions/sysdatetimeoffset-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SYSTEM_USER](https://docs.microsoft.com/sql/t-sql/functions/system-user-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SYSUTCDATETIME]https://docs.microsoft.com/sql/t-sql/functions/sysutcdatetime-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [TAN](https://docs.microsoft.com/sql/t-sql/functions/tan-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [TERTIARY_WEIGHTS](https://docs.microsoft.com/sql/t-sql/functions/collation-functions-tertiary-weights-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [TIMEFROMPARTS](https://docs.microsoft.com/sql/t-sql/functions/timefromparts-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [TODATETIMEOFFSET](https://docs.microsoft.com/sql/t-sql/functions/todatetimeoffset-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [TYPE_ID](https://docs.microsoft.com/sql/t-sql/functions/type-id-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [TYPE_NAME](https://docs.microsoft.com/sql/t-sql/functions/type-name-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [TYPEPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/typeproperty-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [UNICODE](https://docs.microsoft.com/sql/t-sql/functions/unicode-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [UPPER](https://docs.microsoft.com/sql/t-sql/functions/upper-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [USER](https://docs.microsoft.com/sql/t-sql/functions/user-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [USER_NAME](https://docs.microsoft.com/sql/t-sql/functions/user-name-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [VAR](https://docs.microsoft.com/sql/t-sql/functions/var-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [VARP](https://docs.microsoft.com/sql/t-sql/functions/varp-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [YEAR](https://docs.microsoft.com/sql/t-sql/functions/year-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [XACT_STATE](https://docs.microsoft.com/sql/t-sql/functions/xact-state-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="transactions"></a>事务

* [事务](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-sql-data-warehouse?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="diagnostic-sessions"></a>诊断会话

* [CREATE DIAGNOSTICS SESSION](https://docs.microsoft.com/sql/t-sql/language-elements/create-diagnostics-session-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="procedures"></a>过程

* [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_columns](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-columns-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_datatype_info_90](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-datatype-info-90-sql-data-warehouse?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_execute](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latestl)
* [sp_executesql](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_fkeys](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-fkeys-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_pdw_add_network_credentials](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_pdw_database_encryption](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_pdw_database_encryption_regenerate_system_keys](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_pdw_log_user_data_masking](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_pdw_remove_network_credentials](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_pkeys](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-pkeys-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_prepare](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-prepare-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_special_columns_100](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-special-columns-100-sql-data-warehouse?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_sproc_columns](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-sproc-columns-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_statistics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-statistics-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_tables](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-tables-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sp_unprepare](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-unprepare-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="set-statements"></a>SET 语句

* [SET ANSI_DEFAULTS](https://docs.microsoft.com/sql/t-sql/statements/set-ansi-defaults-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET ANSI_NULL_DFLT_OFF](https://docs.microsoft.com/sql/t-sql/statements/set-ansi-null-dflt-off-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET ANSI_NULL_DFLT_ON](https://docs.microsoft.com/sql/t-sql/statements/set-ansi-null-dflt-on-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET ANSI_NULLS](https://docs.microsoft.com/sql/t-sql/statements/set-ansi-nulls-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET ANSI_PADDING](https://docs.microsoft.com/sql/t-sql/statements/set-ansi-padding-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET ANSI_WARNINGS](https://docs.microsoft.com/sql/t-sql/statements/set-ansi-warnings-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET ARITHABORT](https://docs.microsoft.com/sql/t-sql/statements/set-arithabort-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET ARITHIGNORE](https://docs.microsoft.com/sql/t-sql/statements/set-arithignore-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET CONCAT_NULL_YIELDS_NULL](https://docs.microsoft.com/sql/t-sql/statements/set-concat-null-yields-null-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET DATEFIRST](https://docs.microsoft.com/sql/t-sql/statements/set-datefirst-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET DATEFORMAT](https://docs.microsoft.com/sql/t-sql/statements/set-dateformat-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latestl)
* [SET FMTONLY](https://docs.microsoft.com/sql/t-sql/statements/set-fmtonly-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET IMPLICIT_TRANSACITONS](https://docs.microsoft.com/sql/t-sql/statements/set-implicit-transactions-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET LOCK_TIMEOUT](https://docs.microsoft.com/sql/t-sql/statements/set-lock-timeout-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET NUMBERIC_ROUNDABORT](https://docs.microsoft.com/sql/t-sql/statements/set-numeric-roundabort-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET QUOTED_IDENTIFIER](https://docs.microsoft.com/sql/t-sql/statements/set-quoted-identifier-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET ROWCOUNT](https://docs.microsoft.com/sql/t-sql/statements/set-rowcount-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET TEXTSIZE](https://docs.microsoft.com/sql/t-sql/statements/set-textsize-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET TRANSACTION ISOLATION LEVEL](https://docs.microsoft.com/sql/t-sql/statements/set-transaction-isolation-level-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [SET XACT_ABORT](https://docs.microsoft.com/sql/t-sql/statements/set-xact-abort-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="next-steps"></a>后续步骤

有关更多参考信息，请参阅 [Synapse SQL 池中的 T-SQL 语句](sql-data-warehouse-reference-tsql-statements.md)和 [Synapse SQL 池中的系统视图](sql-data-warehouse-reference-tsql-system-views.md)。