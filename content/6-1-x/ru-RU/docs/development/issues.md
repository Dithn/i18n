# Задачи в Electron

# Задачи

* [Как внести вклад в задачи](#how-to-contribute-in-issues)
* [Просьба об общей помощи](#asking-for-general-help)
* [Отправить отчет об ошибке](#submitting-a-bug-report)
* [Прохождение сообщения об ошибке](#triaging-a-bug-report)
* [Разрешение отчета об ошибке](#resolving-a-bug-report)

## Как внести вклад в задачи

Для любого вопроса есть три основных способа, которыми человек может сделать вклад:

1. Открывая вопрос для обсуждения: Если вы считаете, что вы нашли новую ошибку в Electron, вы должны сообщить об этом, создав новую проблему в `electron/electron` трекере.
2. Помочь воспроизвести проблему: Вы можете сделать это либо предоставив вспомогательную информацию (воспроизводимый тестовый случай, который демонстрирует ошибку), либо предложив решить проблему.
3. Помочь решить проблему: это можно сделать, продемонстрировав что проблема не является ошибкой или исправлена; но чаще, открыв запрос на слияние, который изменяет исходник в `electron/electron` в конкретный и рецензируемый способ.

## Просьба об общей помощи

["Finding Support"](../tutorial/support.md#finding-support) имеет список ресурсов для получения помощи по программированию, сообщения о проблемах безопасности, и многое другое. Пожалуйста, используйте трекер ошибок только для ошибок!

## Отправить отчет об ошибке

При открытии новой проблемы в `electron/electron` трекере пользователям будет представлен шаблон, который должен быть заполнен.

```markdown<!--
Спасибо за открытие проблемы! Имейте в виду:

- Трекер отслеживания ошибок предназначен только для ошибок и запросов функций.
- Прежде чем сообщать об ошибке, попробуйте повторить вашу проблему на
  последней версии Electron.
- Если вам нужен общий совет, присоединяйтесь к нашему Slack: http://atom-slack.herokuapp.сom
-->* Версия Electron:
* Операционная система:

### Ожидаемое поведение

<! - Как вы думаете, что должно произойти? -->

### Фактическое поведение<!-- Что на самом деле происходит? -->### Как воспроизвести

<! -

Ваш лучший шанс быстро увидеть эту ошибку - это клонировать РЕПОЗИТОРИЙ и запустить.

Вы можете сделать форк https://github.com/electron/electron-quick-start и включить ссылку на ветку с вашими изменениями.

Если вы указали URL, пожалуйста, перечислите команды, необходимые для клонирования/настройки/запуска вашего репозитория и т.д.

  $ git clone $YOUR_URL -b $BRANCH
  $ npm install
  $ npm start || electron .

-->
```

Если вы считаете, что обнаружили ошибку в Electron, пожалуйста, заполните эту форму в меру ваших возможностей.

Двумя наиболее важными частями информации, необходимой для оценки отчета, являются описание ошибки и простой тестовый случай для его создания. It easier to fix a bug if it can be reproduced.

Смотрите [Как создать минимальный, завершенный и проверяемый пример](https://stackoverflow.com/help/mcve).

## Прохождение сообщения об ошибке

Чаще всего открытые вопросы могут включать в себя обсуждение. Некоторые участники могут иметь различные мнения, в том числе, является ли поведение ошибкой или функцией. Эта дискуссия является частью процесса и должна быть целенаправленной, полезной, и профессиональной.

Краткие ответы, которые не предоставляют ни дополнительного контекста, ни вспомогательных деталей не полезны или не профессиональны. Многих такие ответы раздражают и не дружественны.

Contributors are encouraged to solve issues collaboratively and help one another make progress. If encounter an issue that you feel is invalid, or which contains incorrect information, explain *why* you feel that way with additional supporting context, and be willing to be convinced that you may be wrong. При этом мы часто можем достичь правильного результата быстрее.

## Разрешение отчета об ошибке

Большинство проблем решается путем открытия pull-запроса. The process for opening and reviewing a pull request is similar to that of opening and triaging issues, but carries with it a necessary review and approval workflow that ensures that the proposed changes meet the minimal quality and functional guidelines of the Electron project.