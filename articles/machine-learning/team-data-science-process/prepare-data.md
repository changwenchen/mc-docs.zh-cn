---
title: 准备用于机器学习工作室（经典）的数据 - Team Data Science Process
description: 预处理和清理数据，使其有效地用于机器学习。
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 56f879c40333cabc916fa05243ae3b12113779b0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75599652"
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>用于准备数据以进行增强型机器学习的任务
预处理和清理数据是重要的任务，通常必须先执行此任务才能有效地使用数据集进行机器学习。 原始数据通常具有干扰性且不可靠，还可能缺少值。 使用此类数据进行建模会产生误导性结果。 这些任务是 Team Data Science Process (TDSP) 的一部分，通常对用于发现和计划所需预处理的数据集进行初步探索。 有关 TDSP 过程的详细说明，请参阅 [Team Data Science Process](overview.md) 中概述的步骤。

根据数据存储位置及其格式，可在各种环境（如 SQL、Hive、或 Azure 机器学习工作室（经典））中，使用各种工具和语言（如 R 或 Python）执行预处理和清理任务（如数据浏览任务）。 由于 TDSP 本质上是迭代的，所以这些任务可以在进程工作流中的各个步骤发生。

本文介绍各种数据处理概念，以及可在将数据引入到 Azure 机器学习工作室（经典）之前或之后执行的任务。


## <a name="why-pre-process-and-clean-data"></a>为什么要预处理并清理数据？
实际数据从各种源和进程收集，可能包含违规行为或破坏数，进而影响数据集的之类。 通常引起的数据质量问题包括：

* **不完整**：数据缺少属性或包含缺失值。
* **干扰**：数据包含错误记录或离群值。
* **不一致**：数据包含冲突的记录或差异。

优质数据是优质预测模型的先决条件。 为了避免“无用输入、无用输出”和提高数据质量并进而提高模型性能，必须执行数据运行状况筛查，提前发现数据问题，并确定相应的数据处理和清理步骤。

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>通常执行哪些数据运行状况筛查？
可通过检查以下内容来检查数据的总体质量：

* **记录**的数量。
* **特性**（或**特征**）的数量。
* **数据类型**特性（称名、等级或连续）。
* **缺失值**的数量。
* 数据的**格式正确性**。
  * 如果数据格式为 TSV 或 CSV，请检查列分隔符和行分隔符是否始终正确地分隔列和行。
  * 如果数据格式为 HTML 或 XML，请检查数据是否根据各自的标准保持正确格式。
  * 若要从半结构化或非结构化数据提取结构化信息，可能还需要进行分析。
* **不一致的数据记录**。 查看允许的值范围。 例如，如果数据包含学生 GPA，请检查 GPA 是否在指定范围内（例如 0〜4）。

查找数据问题时，需要执行**处理步骤**，通常涉及清理缺失值、数据规范化、离散化、文本处理，以删除和/或替换可能影响数据对齐的嵌入字符、公共字段中的混合数据类型及其他内容。

**Azure 机器学习使用格式正确的表格数据**。  如果数据已经是表格形式，则可在机器学习中直接使用 Azure 机器学习工作室（经典）执行数据预处理。  如果数据不是表格形式（如 XML），可能需要进行分析以将数据转换为表格形式。  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>数据预处理包含有哪些主要任务？
* **数据清理**：填写缺失值，检测并删除干扰数据和离群值。
* **数据转换**：规范化数据以降低维数和干扰。
* **数据减少**：对数据记录或属性进行采样，便于数据处理。
* **数据离散化**：将连续属性转换为分类属性，便于使用某些机器学习方法。
* **文字清理**：删除可能导致数据未对齐的嵌入字符，例如，制表符分隔的数据文件中的嵌入制表符和可能破坏记录的嵌入式新行等。

以下部分详细介绍一些数据处理步骤。

## <a name="how-to-deal-with-missing-values"></a>如何处理缺失值？
若要处理缺失值，最好先确定缺少值的原因以更好地解决问题。 通常的缺失值处理方法包括：

* **删除**：删除具有缺失值的记录
* **虚拟替换**：使用虚拟值替换缺失值：例如，用 unknown  替换分类值，或用 0 替换数值。
* **平均值替换**：如果缺失的数据是数字，则使用平均值替换缺失值。
* **常用项替换**：如果缺失的是分类数据，则使用最常用的项替换缺失值
* **回归替换**：使用回归方法，将缺失值替换为回归值。  

## <a name="how-to-normalize-data"></a>如何规范化数据？
数据规范化将数值重新调整到指定范围。 常用的数据规范化方法包括：

* **最小 - 最大值规范化**：将数据线性转换到某一范围（例如 0 和 1 之间），这会最小值缩放为 0，最大值缩放为 1。
* **Z 分数规范化**：基于平均值和标准偏差缩放数据：将数据和平均值之间的差除以标准偏差。
* **小数缩放**：通过移动属性值的小数点缩放数据。  

## <a name="how-to-discretize-data"></a>如何离散化数据？
可通过将连续值转换为标称属性或间隔值来离散化数据。 执行此操作的方法包括：

* **等宽分箱**：将属性的所有可能值范围划分为 N 个大小相同的组，并分配具有箱编号的箱中的值。
* **等高分箱**：将属性的所有可能值范围划分为 N 个组，每个组包含相同数量的实例，然后分配具有箱编号的箱中的值。  

## <a name="how-to-reduce-data"></a>如何减少数据？
可通过多种方法缩减数据大小，以便简化数据处理。 根据数据大小和域，可使用以下方法：

* **记录采样**：对数据记录进行采样，并且仅从数据中选择代表性子集。
* **属性采样**：仅从数据中选择最重要属性的子集。  
* **聚合**：将数据分组并存储每个组的编号。 例如，可将过去 20 年餐饮连锁的每日收入聚合为每月收入，从而缩减数据大小。  

## <a name="how-to-clean-text-data"></a>如何清理文本数据？
**表格数据中的文本字段**可能包含影响列对齐和/或记录边界的字符。 例如，制表符分隔的文件中的嵌入式制表符会导致列不对齐，而嵌入式新行字符会破坏记录行。 写入或读取文本时，若处理文本编码的方式不当会导致信息丢失、意外引入不可读字符（例如，null），还可能影响文本分析。 若要清理文本字段以确保正确对齐和/或从非结构化或半结构化文本数据中提取结构化数据，可能需要进行仔细的分析和编辑。

**数据浏览**支持提前预览数据。 执行该步骤时会发现多个数据问题，可应用相应的方法解决这些问题。  提问非常重要，例如问题的源是什么以及问题是如何引入的。 这还有助于确定需采取哪些数据处理步骤来解决这些问题。 从数据中获得的见解也可用于确定数据处理操作的优先级。

## <a name="references"></a>参考
> 《数据挖掘：概念和技术》  ，第三版，Morgan Kaufmann 出版社，2011，Jiawei Han、Micheline Kamber 和 Jian Pei
> 
> 

