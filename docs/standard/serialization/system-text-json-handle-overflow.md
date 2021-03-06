---
title: Как обрабатывать переполнения JSON с помощью System.Text.Json
description: Узнайте, как в .NET обрабатывать переполнения JSON при сериализации в JSON и десериализации из JSON.
ms.date: 11/30/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 265ce4f77d353720419122d17c36e508a377b68f
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008920"
---
# <a name="how-to-handle-overflow-json-with-no-locsystemtextjson"></a>Как обрабатывать переполнения JSON с помощью System.Text.Json

Из этой статьи вы узнаете, как обрабатывать переполнения JSON с помощью пространства имен `System.Text.Json`.

## <a name="handle-overflow-json"></a>Обработка переполнения JSON

При десериализации в JSON могут быть получены данные, которые не представлены свойствами целевого типа. Предположим у вас есть следующий целевой тип:

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WF":::

А десериализируемым кодом JSON является:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

При десериализации кода JSON, показанного в указанном типе, свойства `DatesAvailable` и `SummaryWords` никуда не переходят и будут потеряны. Для записи дополнительных данных, таких как эти свойства, примените атрибут [[JsonExtensionData]](xref:System.Text.Json.Serialization.JsonExtensionDataAttribute) к свойству типа `Dictionary<string,object>` или `Dictionary<string,JsonElement>`:

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithExtensionData":::

При десериализации показанного ранее JSON в этот тип данных дополнительные данные становятся парами "ключ-значение" свойства `ExtensionData`:

| Свойство. | Значение | Примечания |
|--|--|--|
| `Date` | `"8/1/2019 12:00:00 AM -07:00"` |  |
| `TemperatureCelsius` | `0` | Несовпадение с учетом регистра (`temperatureCelsius` в коде JSON), поэтому свойство не задано. |
| `Summary` | `"Hot"` |  |
| `ExtensionData` | `temperatureCelsius: 25` | Так как регистр не совпадает, это свойство JSON является лишним и становится парой "ключ-значение" в словаре. |
| `DatesAvailable` | `[ "8/1/2019 12:00:00 AM -07:00", "8/2/2019 12:00:00 AM -07:00" ]` | Дополнительное свойство из кода JSON преобразуется в пару "ключ-значение" с массивом в качестве объекта значения. |
| `SummaryWords` | `[ "Cool", "Windy", "Humid" ]` | Дополнительное свойство из кода JSON преобразуется в пару "ключ-значение" с массивом в качестве объекта значения. |

Когда целевой объект сериализуется, пары "ключ-значение" данных расширения становятся свойствами JSON точно так же, как они были во входящем коде JSON:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 0,
  "Summary": "Hot",
  "temperatureCelsius": 25,
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

Обратите внимание, что имя свойства `ExtensionData` не отображается в коде JSON. Такое поведение позволяет JSON выполнить круговой путь без потери дополнительных данных, которые в противном случае не будут десериализованы.

## <a name="see-also"></a>См. также раздел

* [Общие сведения о System.Text.Json](system-text-json-overview.md)
* [Практическое руководство. Сериализация и десериализация JSON](system-text-json-how-to.md)
* [Создание экземпляров JsonSerializerOptions](system-text-json-configure-options.md)
* [Сопоставление без учета регистра](system-text-json-character-casing.md)
* [Настройка имен и значений свойств](system-text-json-customize-properties.md)
* [Игнорирование свойств](system-text-json-ignore-properties.md)
* [Применение недействительного кода JSON](system-text-json-invalid-json.md)
* [Сохранение ссылок](system-text-json-preserve-references.md)
* [Неизменяемые типы и непубличные методы доступа](system-text-json-immutability.md)
* [Полиморфная сериализация](system-text-json-polymorphism.md)
* [Миграция из Newtonsoft.Json в System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [Настройка кодировки символов](system-text-json-character-encoding.md)
* [Написание пользовательских сериализаторов и десериализаторов](write-custom-serializer-deserializer.md)
* [Написание пользовательских преобразователей для сериализации JSON](system-text-json-converters-how-to.md)
* [Поддержка DateTime и DateTimeOffset](../datetime/system-text-json-support.md)
* [Справочник по API System.Text.Json](xref:System.Text.Json)
* [Справочник по API System.Text.Json.Serialization](xref:System.Text.Json.Serialization)
