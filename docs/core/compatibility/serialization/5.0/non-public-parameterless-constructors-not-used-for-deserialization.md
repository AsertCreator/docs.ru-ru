---
title: Критическое изменение. Не являющиеся открытыми конструкторы без параметров не используются для десериализации
description: Узнайте о критическом изменении в .NET 5.0, где не являющиеся открытыми конструкторы без параметров больше не используются для десериализации с помощью JsonSerializer.
ms.date: 10/18/2020
ms.openlocfilehash: 6bdcc92c61008aa4ee27370bbac4dbf4ee3ef7c8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759967"
---
# <a name="non-public-parameterless-constructors-not-used-for-deserialization"></a>Не являющиеся открытыми конструкторы без параметров не используются для десериализации

Чтобы гарантировать согласованность, во всех поддерживаемых моникерах целевой платформы (TFM) не являющиеся открытыми конструкторы без параметров больше не используются для десериализации с помощью <xref:System.Text.Json.JsonSerializer> по умолчанию.

## <a name="change-description"></a>Описание изменений

Поведение отдельных [пакетов NuGet System.Text.Json](https://www.nuget.org/packages/System.Text.Json/), которые поддерживают .NET Standard 2.0 и более поздних версий (версии с 4.6.0 по 4.7.2), согласуется со встроенным поведением в .NET Core 3.0 и 3.1. В .NET Core 3. x внутренние и закрытые конструкторы можно использовать для десериализации. В отдельных пакетах не допускаются конструкторы, не являющиеся открытыми, и создается исключение <xref:System.MissingMethodException>, если не определен открытый конструктор без параметров.

Начиная с .NET 5.0 и версии 5.0.0 пакета NuGet System.Text.Json согласовано поведение между пакетом NuGet и встроенными API. Сериализатор по умолчанию игнорирует не являющиеся открытыми конструкторы, в том числе конструкторы без параметров. Сериализатор использует для десериализации один из следующих конструкторов:

- Открытый конструктор, помеченный атрибутом <xref:System.Text.Json.Serialization.JsonConstructorAttribute>.
- Открытый конструктор без параметров.
- Открытый параметризованный конструктор (если это единственный открытый конструктор).

Если ни один из этих конструкторов не доступен, при попытке десериализовать тип возникает исключение <xref:System.NotSupportedException>.

## <a name="version-introduced"></a>Представленная версия

5.0

## <a name="reason-for-change"></a>Причина изменения

- Принудительно обеспечить согласованное поведение между всеми моникерами целевой платформы (TFM), для которых создается <xref:System.Text.Json?displayProperty=fullName> (.NET Core 3.0 и более поздних версий и .NET Standard 2.0)
- Поскольку <xref:System.Text.Json.JsonSerializer> не должен выполнять вызовы к не являющейся открытой контактной зоне типа, будь то конструктор, свойство или поле.

## <a name="recommended-action"></a>Рекомендованное действие

- Если вы являетесь владельцем типа и это целесообразно, сделайте конструктор без параметров открытым.
- В противном случае реализуйте для типа `JsonConverter<T>` и обеспечьте контроль десериализации.

## <a name="affected-apis"></a>Затронутые API

- <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=fullName>
- <xref:System.Text.Json.JsonSerializer.DeserializeAsync%2A?displayProperty=fullName>

<!--

### Affected APIs

- `Overload:System.Text.Json.JsonSerializer.Deserialize`
- `Overload:System.Text.Json.JsonSerializer.DeserializeAsync`

### Category

Serialization

-->
