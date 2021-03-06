---
title: Предопределенные файлы конфигурации (анализ кода)
description: Узнайте, как использовать предопределенные файлы editorconfig и наборов правил для назначения конкретных типов анализа кода.
ms.date: 09/24/2020
ms.topic: conceptual
ms.openlocfilehash: 4937dcab1183fa3f63be4afc71627a7c7c08c6bd
ms.sourcegitcommit: 665f8fc55258356f4d2f4a6585b750c974b26675
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "96593137"
---
# <a name="predefined-configuration-files"></a>Предопределенные файлы конфигурации

Доступны предопределенные файлы EditorConfig и [наборов правил](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules) , которые позволяют быстро и легко включить категорию правил качества кода, например правила безопасности или разработки. Включив определенную категорию правил, можно вычислить целевые проблемы и конкретные условия. Чтобы получить доступ к этим предопределенным файлам, установите пакет анализатора NuGet [Microsoft. CodeAnalysis. нетанализерс](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) .

[Microsoft. CodeAnalysis. нетанализерс](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) включает предопределенные файлы EditorConfig и наборы правил для следующих категорий правил:

- Все правила
- Поток данных
- Конструирование
- Документация
- Глобализация
- Совместимость
- Удобство обслуживания
- Именование
- Производительность
- Перенесено из FxCop
- надежность;
- Безопасность
- Использование

Каждая из этих категорий правил имеет файл EditorConfig или набора правил:

- Включить все правила в категории (и отключить все остальные правила)
- Используйте уровень серьезности по умолчанию для каждого правила и параметр включен по умолчанию (и отключите все остальные правила).

> [!TIP]
> Категория "все правила" имеет дополнительный файл EditorConfig или набора правил для отключения всех правил. Используйте этот файл, чтобы быстро избавиться от любых предупреждений или ошибок анализатора в проекте.

## <a name="predefined-editorconfig-files"></a>Предопределенные файлы EditorConfig

Предопределенные файлы EditorConfig для пакета анализатора Microsoft. CodeAnalysis. Нетанализерс находятся в подкаталоге *EditorConfig* , где установлен пакет NuGet. Например, файл EditorConfig для включения всех правил безопасности находится в папке *EditorConfig/секуритирулесенаблед/. EditorConfig*.

Скопируйте выбранный файл *. editorconfig* в корневой каталог проекта.

## <a name="predefined-rule-sets"></a>Предопределенные наборы правил

Стандартные файлы набора правил для пакета анализатора Microsoft. CodeAnalysis. Нетанализерс находятся в подкаталоге *набора правил* , в котором установлен пакет NuGet. Например, файл набора правил для включения всех правил безопасности находится в папке *RuleSets/секуритирулесенаблед. RuleSet*. Скопируйте один или несколько наборов правил и вставьте их в каталог, содержащий проект.

## <a name="see-also"></a>См. также

- [Конфигурация анализатора](https://github.com/dotnet/roslyn-analyzers/blob/master/docs/Analyzer%20Configuration.md)
- [Параметры правила стиля кода .NET для EditorConfig](code-style-rule-options.md)
