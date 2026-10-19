---

### Вариант 2: Компактный инженерный (ближе к реальному GitHub-репозиторию)

```markdown
# Лабораторная работа № 1: Тест-планирование платформы BookSphere с использованием LLM

В репозитории собраны артефакты первой лабораторной работы по проектированию тестовой документации для сервиса **BookSphere**. Основная задача исследования — оценить качество генерации тест-планов различными языковыми моделями (DeepSeek, ChatGPT 6, Claude Sonnet 5) по трем индустриальным шаблонам, провести сравнительный разбор и сформировать результирующий пакет документации.

---

## Быстрые ссылки

| Раздел | Описание и ссылки |
| :--- | :--- |
| **О проекте** | [Спецификация BookSphere (task.md)](task.md) • [Состав команды (authors.md)](authors.md) |
| **Индивидуальные этапы** | [Ветка Студента 1 (Qwen)](work/student1/readme.md) • [Ветка Студента 2 (Google Gemini 4 Argon)](work/student2/readme.md) • [Ветка Студента 3 (DeepSeek)](work/student3/readme.md) |
| **Финальные артефакты** | [Сводный анализ, матрица сравнения и итоговые тест-планы (results/readme.md)](results/readme.md) |

## Используемые стандарты документации

1. **IEEE 829-1998** — международный стандарт документации тестирования ПО ([открыть PDF](docs/TestPlanTemplate_IEEE_829-1998.pdf)).
2. **RUP (Rational Unified Process)** — итеративный шаблон планирования тестов ([открыть PDF](docs/TestPlanTemplate_RUP.pdf)).
3. **Локализованный стандарт (РФ)** — типовой регламент плана тестирования ([открыть PDF](docs/План_тестирования.pdf)).

---

## План работы над проектом

* **Шаг 1. Постановка задачи:** декомпозиция требований к платформе BookSphere на базе единого паспорта продукта и изучение структуры трех базовых шаблонов.
* **Шаг 2. Генерация индивидуальных планов:** каждый участник проработал цепочки промптов в своей LLM для создания трех тест-планов. Дампы переписок сохранены в `work/studentN/prompt_history/`, а готовые черновики — в `work/studentN/plans/`.
* **Шаг 3. Кросс-ревью (A/B/C):** для каждого из трех шаблонов проведено слепое сравнение версий A, B и C. Обоснование плюсов и минусов каждого варианта вынесено в `results/conclusions/`, ключ соответствия моделей находится в `results/readme.md`.
* **Шаг 4. Компиляция эталонов:** на основе выводов ревью сформированы три финальных документа в каталоге `results/plans/`.
* **Шаг 5. Релиз репозитория:** верификация ссылок, формирование файла структуры и оформление документации.

---

## Карта файловой системы

```text
repos/
  results/
    conclusions/
      analysis_TestPlanTemplate_IEEE_829-1998.md
      analysis_plan_TestPlanTemplate_RUP.md
      analysis_plan_standart3.md
    plans/
      plan_TestPlanTemplate_IEEE_829-1998.md
      plan_TestPlanTemplate_RUP.md
      plan_План_тестирования.md
    readme.md
  standarts/
    TestPlanTemplate_IEEE_829-1998.pdf
    TestPlanTemplate_RUP.pdf
    План тестирования.doc
    План тестирования.pdf
  work/
    student1/
      plans/
        plan_TestPlanTemplate_IEEE_829-1998.md
        plan_TestPlanTemplate_RUP.md
        plan_План_тестирования.md
      prompt_history/
        prompt_history_TestPlanTemplate_IEEE_829-1998.md
        prompt_history_TestPlanTemplate_RUP.md
        prompt_history_План_тестирования.md
      readme.md
    student2/
      plans/
        plan_TestPlanTemplate_IEEE_829-1998.md
        plan_TestPlanTemplate_RUP.md
        plan_План_тестирования.md
      prompt_history/
        prompt_history_TestPlanTemplate_IEEE_829-1998.md
        prompt_history_TestPlanTemplate_RUP.md
        prompt_history_План_тестирования.md
      readme.md
    student3/
      plans/
        plan_TestPlanTemplate_IEEE_829-1998.md
        plan_TestPlanTemplate_RUP.md
        plan_План_тестирования.md
      prompt_history/
        prompt_history_TestPlanTemplate_IEEE_829-1998.md
        prompt_history_TestPlanTemplate_RUP.md
        prompt_history_План_тестирования.md
      readme.md
  authors.md
  readme.md
  structura.txt
  task.md