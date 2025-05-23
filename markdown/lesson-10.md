## Урок 10. AI-агенты в продакшене

### Введение

Этот урок охватывает:

* Как эффективно планировать развертывание AI-агента в продакшене.
* Распространённые ошибки и проблемы, с которыми можно столкнуться при развертывании AI-агента.
* Как управлять затратами, сохраняя при этом производительность AI-агента.

### Цели обучения

После завершения урока вы будете знать и понимать:

* Приёмы повышения производительности, эффективности и снижения затрат в продакшен-системах AI-агентов.
* Что и как нужно оценивать в работе AI-агентов.
* Как контролировать затраты при выводе AI-агентов в продакшен.

> **Важно**: Важно разрабатывать AI-агентов, которым можно доверять. См. урок "Создание надёжных AI-агентов".

---

### Оценка AI-агентов

До, во время и после развертывания AI-агентов критически важно иметь систему оценки. Это позволяет убедиться, что система соответствует вашим целям и целям пользователей.

Для полноценной оценки AI-агента необходимо анализировать не только его ответы, но и всю систему, в которой он работает. Важно учитывать:

* Начальный запрос к модели.
* Способность агента распознавать намерение пользователя.
* Способность агента выбрать подходящий инструмент для выполнения задачи.
* Ответ инструмента на запрос агента.
* Способность агента интерпретировать ответ инструмента.
* Обратную связь пользователя на ответ агента.

Такой модульный подход позволяет точнее находить зоны для улучшения и отслеживать влияние изменений в моделях, промптах, инструментах и других компонентах.

---

### Частые проблемы и возможные решения в работе AI-агентов

| Проблема                                              | Возможное решение                                                                                                                                                                                |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Агент не выполняет задачи стабильно                   | - Уточните промпт, чётко формулируйте цели.<br>- Разделите задачу на подзадачи и распределите между несколькими агентами.                                                                        |
| Агент зацикливается                                   | - Определите чёткие условия завершения процесса.<br>- Используйте более крупные модели, способные к рассуждению и планированию.                                                                  |
| Инструменты, вызываемые агентом, работают некорректно | - Тестируйте и проверяйте инструменты отдельно от системы агентов.<br>- Уточняйте параметры, промпты и названия инструментов.                                                                    |
| Мультиагентная система работает нестабильно           | - Уточните промпты для каждого агента, чтобы они были специфичны и не перекрывались.<br>- Постройте иерархическую систему с управляющим агентом, который будет направлять задачи нужным агентам. |

---

### Управление затратами

Вот несколько стратегий для оптимизации затрат при развертывании AI-агентов в продакшен:

**Кэширование ответов** — выделите часто повторяющиеся запросы и заранее подготовьте ответы, минуя агентную систему. Можно также реализовать поток, определяющий степень схожести запроса с кэшированными, используя более простые модели.

**Использование меньших моделей** — маленькие языковые модели (SLM) хорошо справляются с рядом задач и существенно снижают расходы. Оцените эффективность работы SLM по сравнению с более крупными моделями в вашем случае.

**Маршрутизация с помощью моделей** — комбинируйте модели разных размеров. Используйте SLM/LLM или серверлес-функции, чтобы направлять задачи по сложности к подходящей модели. Это снижает затраты и сохраняет нужный уровень производительности.
