# Урок 4: Tool Use Design Pattern

## Введение

**Вопросы урока:**

* Что такое Tool Use Design Pattern?
* В каких случаях он применяется?
* Какие элементы необходимы для его реализации?
* Какие особенности нужно учитывать при построении доверенных AI-агентов с использованием этого паттерна?

**Цели обучения:**

* Определить назначение Tool Use Design Pattern.
* Выявить подходящие сценарии использования.
* Понять ключевые элементы реализации паттерна.
* Распознать аспекты доверия при использовании этого подхода.

## Что такое Tool Use Design Pattern?

Tool Use Design Pattern предоставляет LLM-агентам возможность взаимодействовать с внешними инструментами для достижения целей. Инструменты — это код, который агент может исполнять: от простых функций (например, калькулятор) до вызовов API сторонних сервисов (например, получения погоды).

## Применение паттерна

* **Динамическое извлечение информации:** доступ к базам данных или API.
* **Выполнение и интерпретация кода:** математические расчёты, генерация отчётов.
* **Автоматизация рабочих процессов:** задачи, письма, пайплайны.
* **Поддержка пользователей:** доступ к CRM, тикет-системам, базам знаний.
* **Генерация и редактирование контента:** проверка грамматики, суммаризация, фильтрация.

## Основные элементы реализации

* **Function/Tool Calling:** основной механизм взаимодействия агента с инструментами.
* **Schema:** описание доступных функций с параметрами.
* **Code:** реализация каждой функции.
* **LLM, поддерживающий function calling.**

### Пример: Получение времени в городе

1. **Инициализация клиента Azure OpenAI:**

```python
client = AzureOpenAI(
    azure_endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
    api_key=os.getenv("AZURE_OPENAI_API_KEY"),
    api_version="2024-05-01-preview"
)
```

2. **Описание функции:**

```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_time",
            "description": "Get the current time in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city name, e.g. San Francisco"
                    }
                },
                "required": ["location"]
            }
        }
    }
]
```

3. **Вызов модели:**

```python
messages = [{"role": "user", "content": "What's the current time in San Francisco"}]
response = client.chat.completions.create(
    model=deployment_name,
    messages=messages,
    tools=tools,
    tool_choice="auto",
)
```

4. **Реализация функции:**

```python
def get_current_time(location):
    ... # возвращает текущее время в городе
```

5. **Обработка tool\_call:**

```python
if response_message.tool_calls:
    ... # вызвать функцию, добавить результат в messages
```

6. **Повторный вызов модели для финального ответа.**

## Поддержка в agentic-фреймворках

### Semantic Kernel

* Упрощает создание schema через сериализацию функций.
* Поддерживает плагины (plugins) с декораторами `@kernel_function`.

```python
class GetCurrentTimePlugin:
    @kernel_function(description="Get the current time for a given location")
    def get_current_time(location: str = ""):
        ...
```

### Azure AI Agent Service

* Полностью управляемый сервис с безопасностью уровня enterprise.
* Автоматически обрабатывает вызовы инструментов и управляет состоянием диалога через threads.
* Имеет предустановленные инструменты:

  * **Knowledge tools:** Bing Search, Azure AI Search, File Search
  * **Action tools:** Function Calling, Code Interpreter, Azure Functions

**Пример инициализации:**

```python
project_client = AIProjectClient.from_connection_string(...)
toolset = ToolSet()
toolset.add(FunctionTool(fetch_sales_data_using_sqlite_query))
toolset.add(CodeInterpreterTool())
agent = project_client.agents.create_agent(..., toolset=toolset)
```

## Особенности доверия

* Ограничить права доступа к БД (например, только SELECT).
* Запускать агента в изолированной среде.
* Использовать read-only реплики или data warehouse для безопасного доступа.

---

Следующий шаг: практика создания и подключения собственных инструментов к агенту с использованием выбранного фреймворка.
