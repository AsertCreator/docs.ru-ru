---
title: 'Примеры бессерверного проектирования: бессерверные приложения'
description: Узнайте о различных сценариях, поддерживаемых бессерверными архитектурами, — от планирования и обработки событий до триггеров файлов и процесса потоковой передачи.
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 3aa9b7951fd8f11a65a64c22443de7041aba7d94
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91171758"
---
# <a name="serverless-design-examples"></a>Примеры бессерверного проектирования

Существует множество конструктивных шаблонов для бессерверных решений. В этом разделе описываются некоторые распространенные сценарии, в которых используются бессерверные функции. Общим для всех примеров является фундаментальное сочетание триггера событий и бизнес-логики.

## <a name="scheduling"></a>Планирование

Планирование задач — это распространенная функция. На следующей схеме показана устаревшая база данных, для которой не предусмотрены соответствующие проверки целостности. База данных должна периодически очищаться. Бессерверная функция находит недопустимые данные и очищает их. Триггер — это таймер, который запускает код по расписанию.

![Бессерверная функция планирования](./media/serverless-scheduling.png)

## <a name="command-and-query-responsibility-segregation-cqrs"></a>Разделение ответственности команды и запроса (CQRS)

Разделение команд и запросов (CQRS) — это шаблон, предоставляющий различные интерфейсы для чтения (или запроса) данных и операций, изменяющих данные. Он устраняет некоторые распространенные проблемы. В традиционных системах с операциями создания, чтения, изменения и удаления (CRUD) могут возникать конфликты из-за большого объема операций чтения и записи в одно и то же хранилище данных. Может часто возникать блокировка, резко замедляющая операции чтения. Часто данные представляются как соединение нескольких объектов предметной области, и операции чтения должны объединять данные из разных сущностей.

При использовании шаблона CQRS операции чтения могут включать в себя специальный "плоский" объект, который моделирует данные с учетом способа их потребления. Операции чтения обрабатываются не так, как операции хранения. Например, хотя база данных может хранить контакт в виде записи заголовка с дочерней записью адреса, операция чтения может включать в себя сущность со свойствами заголовка и адреса. Есть множество подходов к созданию модели чтения. Их можно реализовать с помощью представлений. Операции обновления можно инкапсулировать как изолированные события, которые затем запускают обновления для двух разных моделей. Для чтения и записи существуют отдельные модели.

![Пример CQRS](./media/cqrs-example.png)

Бессерверная модель может реализовать шаблон CQRS путем предоставления разделенных конечных точек. Одна бессерверная функция может выполнять запросы или операции чтения, а другая бессерверная функция или набор функций может обрабатывать операции обновления. Бессерверная функция может также отвечать за поддержание актуальности модели чтения и может запускаться [каналом изменений базы данных](/azure/cosmos-db/change-feed). Интерфейсная разработка упрощается до подключения к необходимым конечным точкам. Обработка событий выполняется в серверной части. Эта модель также хорошо масштабируется для больших проектов, так как разные команды могут работать над различными операциями.

## <a name="event-based-processing"></a>Обработка на основе событий

В системах, основанных на сообщениях, события часто собираются в очередях или разделах издателя/подписчика, над которыми производятся операции. Эти события могут инициировать бессерверные функции для выполнения части бизнес-логики. Примером обработки на основе событий являются системы с источниками событий. Событие вызывается, чтобы отметить задачу как завершенную. Бессерверная функция, вызванная событием, обновляет соответствующий документ базы данных. Вторая бессерверная функция может использовать событие, чтобы обновить модель чтения для системы. [Служба "Сетка событий Azure"](/azure/event-grid/overview) предоставляет способ интеграции событий с функциями в качестве подписчиков.

> События — это информационные сообщения. Дополнительные сведения см. в статье [Event Sourcing pattern](/azure/architecture/patterns/event-sourcing) (Шаблон источников событий).

## <a name="file-triggers-and-transformations"></a>Триггеры и преобразования файлов

Извлечение, преобразование и загрузка (ETL) — это обычная бизнес-функция. Бессерверная модель является отличным решением для ETL, так как позволяет запускать код как часть конвейера. Отдельные компоненты кода могут отвечать за различные аспекты. Одна бессерверная функция может скачивать файл, другая применять преобразование, а третья загружать данные. Код можно тестировать и развертывать независимо, что упрощает обслуживание и масштабирование по мере необходимости.

![Бессерверные триггеры и преобразования файлов](./media/serverless-file-triggers.png)

На схеме "холодное хранилище" представляет данные, которые анализируются в [Azure Stream Analytics](/azure/stream-analytics). Любые проблемы, возникающие в потоке данных, запускают функцию Azure для устранения аномалии.

## <a name="asynchronous-background-processing-and-messaging"></a>Асинхронная фоновая обработка и обмен сообщениями

Асинхронная фоновая обработка и обмен сообщениями позволяют приложениям запускать процессы без ожидания. Примером асинхронной обработки является приложение распознавания текста (OCR). Изображение отправляется и помещается в очередь для обработки. Сканирование изображения для извлечения текста может занять некоторое время, после чего отправляется уведомление. В этом сценарии бессерверные функции могут обрабатывать как вызов, так и результат.

## <a name="web-apps-and-apis"></a>Веб-приложения и API

Распространенный сценарий применения бессерверных приложений — n-уровневые приложения, в которых чаще всего уровень пользовательского интерфейса является веб-приложением. Популярность одностраничных приложений (SPA) в последнее время резко возросла. Приложения SPA отображают одну страницу, затем используют вызовы API и возвращенные данные для динамического отображения нового пользовательского интерфейса без повторной загрузки полной страницы. При отображении на стороне клиента обеспечивается более высокая скорость и более быстрое реагирование приложения для пользователя.

Бессерверные конечные точки, активируемые HTTP-вызовами, можно использовать для обработки запросов API. Например, рекламная компания может вызвать бессерверную функцию с информацией о профиле пользователя для запроса пользовательской рекламы. Бессерверная функция возвращает пользовательское объявление, и веб-страница отображает его.

![Бессерверный веб-API](./media/serverless-web-api.png)

## <a name="data-pipeline"></a>Конвейер данных

Бессерверные функции можно использовать для упрощения конвейера данных. В этом примере файл активирует функцию для преобразования данных в CSV-файле в строки данных в таблице. Благодаря упорядоченной таблице на панели мониторинга Power BI может быть представлена аналитика пользователю.

![Конвейер бессерверных данных](./media/serverless-data-pipeline.png)

## <a name="stream-processing"></a>Потоковая обработка

Устройства и датчики часто генерируют потоки данных, которые должны обрабатываться в реальном времени. Есть ряд технологий, которые могут записывать сообщения и потоки из [Центров событий](/azure/event-hubs/event-hubs-what-is-event-hubs) и [Центра Интернета вещей](/azure/iot-hub) в [служебную шину](/azure/service-bus). Независимо от способа передачи, бессерверные функции являются идеальным механизмом для обработки сообщений и потоков данных по мере их поступления. Бессерверные функции можно быстро масштабировать при работе с большими объемами данных. Бессерверный код может применять бизнес-логику, чтобы анализировать данные и выводить их в структурированном формате для выполнения действий и получения аналитики.

![Обработка потока с помощью бессерверных функций](./media/serverless-stream-processing.png)

## <a name="api-gateway"></a>Шлюз API

Шлюз API обеспечивает единую точку входа для клиентов, а затем интеллектуально направляет запросы к серверным службам. Это полезно для управления большими наборами служб. Он также может осуществлять управление версиями и упрощать разработку, легко подключая клиентов к различным средам. Бессерверные функции могут работать с внутренним масштабированием отдельных микрослужб, в то же время предоставляя один внешний интерфейс через шлюз API.

![Шлюз API с бессерверными функциями](./media/serverless-api-gateway.png)

## <a name="recommended-resources"></a>Рекомендуемые ресурсы

- [Сетка событий Azure](/azure/event-grid/overview)
- [Центр Интернета вещей Azure](/azure/iot-hub)
- [Распределенное управление данными: проблемы и решения](../microservices/architect-microservice-container-applications/distributed-data-management.md)
- [Identifying microservice boundaries](/azure/architecture/microservices/microservice-boundaries) (Определение границ микрослужбы)
- [Центры событий](/azure/event-hubs/event-hubs-what-is-event-hubs)
- [Event Sourcing pattern](/azure/architecture/patterns/event-sourcing) (Шаблон источников событий)
- [Реализация шаблона размыкателя цепи](../microservices/implement-resilient-applications/implement-circuit-breaker-pattern.md)
- [Центр Интернета вещей](/azure/iot-hub)
- [Служебная шина](/azure/service-bus)
- [Работа с поддержкой веб-канала изменений в Azure Cosmos DB](/azure/cosmos-db/change-feed)

>[!div class="step-by-step"]
>[Назад](serverless-architecture-considerations.md)
>[Вперед](azure-serverless-platform.md)
