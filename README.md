Pairwise Independent Combinatorial Testing
==========================================

PICT generates test cases and test configurations. With PICT, you can generate tests that are more effective than manually generated tests and in a fraction of the time required by hands-on test case design.

PICT runs as a command line tool. Prepare a model file detailing the parameters of the interface (or set of configurations, or data) you want to test. PICT generates a compact set of parameter value choices that represent the test cases you should use to get comprehensive combinatorial coverage of your parameters.

For instance, if you wish to create a test suite for partition and volume creation, the domain can be described by the following parameters: **Type**, **Size**, **File system**, **Format method**, **Cluster size**, and **Compression**. Each parameter has a limited number of possible values, each of which is determined by its nature (for example, **Compression** can only be **On** or **Off**) or by the equivalence partitioning (such as **Size**).

    Type:          Single, Span, Stripe, Mirror, RAID-5
    Size:          10, 100, 500, 1000, 5000, 10000, 40000
    Format method: Quick, Slow
    File system:   FAT, FAT32, NTFS
    Cluster size:  512, 1024, 2048, 4096, 8192, 16384, 32768, 65536
    Compression:   On, Off

There are thousands of possible combinations of these values. It would be  difficult to test all of them in a reasonable amount of time. Instead, we settle on testing all possible pairs of values. For example, **{Single, FAT}** is one pair, **{10, Slow}** is another; one test case can cover many pairs. Research shows that testing all pairs is an effective alternative to exhaustive testing and much less costly. It will provide very good coverage and the number of test cases will remain manageable.

# More information

See **[doc/pict.md](https://github.com/Microsoft/pict/blob/master/doc/pict.md)** for detailed documentation on PICT and http://pairwise.org has details on this testing methododology. 

The most recent **pict.exe** is available at https://github.com/microsoft/pict/releases/download/release/pict.exe.

# Contributing

PICT consists of the following projects:
 * **api**: The core combinatorial engine,
 * **cli**: PICT.EXE command-line tool,
 * **clidll**: PICT.EXE client repackaged as a Windows DLL to be used in-proc,
 * **api-usage**: A sample of how the engine API can be used,
 * **clidll-usage**: A sample of how the PICT DLL is to be used.

## Building and testing on Windows with MsBuild
Use **pict.sln** to open the solution in Visual Studio 2019. You will need VC++ build tools installed. See https://www.visualstudio.com/downloads/ for details.

PICT uses MsBuild for building. **_build.cmd** script in the root directory will build both Debug and Release from the command-line.

The **test** folder contains all that is necessary to test PICT. You need Perl to run the tests. **_test.cmd** is the script that does it all.

The test script produces a log: **dbg.log** or **rel.log** for the Debug and Release bits respectively. Compare them with their committed baselines and make sure all the differences can be explained.

>There are tests which randomize output which typically make it different on each run. These results should be masked in the baseline but currently aren't.

## Building with clang++ on Linux, OS/X, *BSD, etc.
Install clang through your package manager (most systems), Xcode (OS/X), or from the [LLVM website](http://llvm.org/releases/).
On Linux, you also need to install recent GNU's "libstdc++" or LLVM's "libc++".

Run `make` to build the `pict` binary.

Run `make test` to run the tests as described above (requires Perl).

## Debugging

Most commonly, you will want to debug the command-line tool. Start in the **pictcli** project, **cli/pict.cpp** file. You'll find **wmain** routine there which would be a convenient place to put the very first breakpoint.

15.02.26
Ось результати аналізу та стратегія трансформації для проекту **PICT (Pairwise Independent Combinatorial Tool)**, підготовлені у форматі для Notion.

---

# 📑 Звіт AI-консультанта: Проект "PICT"

## 🧬 Частина 1: "ДНК" Проекту

Проект **PICT** — це спеціалізований інструмент для автоматизації проектування тестів, розроблений компанією Microsoft (і форкнутий користувачем MIkeCall1986). Його логіку можна розбити на такі **атомарні функції**:

*   **Парсинг моделі (Model Parsing):** Читання вхідного файлу моделі, який описує параметри інтерфейсу, конфігурації або дані, що потребують тестування.
*   **Комбінаторний рушій (Combinatorial Engine):** Ядро системи (`api`), яке виконує математичні розрахунки для створення мінімального набору тест-кейсів, що покривають усі можливі пари значень параметрів.
*   **Генерація конфігурацій (Configuration Generation):** Побудова компактного набору виборів значень параметрів, що забезпечує всебічне комбінаторне покриття.
*   **Інтерфейсна взаємодія (CLI/DLL Interface):** Надання доступу до функціоналу через командний рядок (`cli`) або динамічну бібліотеку (`clidll`) для вбудовування в інші процеси.
*   **Валідація та тестування:** Перевірка коректності генерації за допомогою Perl-скриптів та порівняння з еталонними логами.

### 💎 Головна технічна цінність
Головна цінність полягає у **радикальному скороченні кількості тестів при збереженні високої якості покриття**. Замість того, щоб тестувати тисячі можливих комбінацій вручну, PICT генерує лише необхідний мінімум (Pairwise Testing), що дозволяє виявляти більшість помилок за частку часу, необхідного для традиційного дизайну тест-кейсів.

---

## 🚀 Частина 2: "Трансформація" (Інтеграція з Gemini LLM)

*Інформація щодо трансформації базується на аналізі технічних можливостей проекту та потенціалу AI (GitHub Copilot, Spark), згаданих у джерелах.*

Додавання Gemini перетворює PICT з інструмента для інженерів на **автономного QA-архітектора**.

### Як зміниться функціонал?
1.  **Генерація моделей з документації:** Користувач завантажує опис функції (наприклад, техзавдання), а Gemini автоматично створює файл моделі PICT із переліком параметрів та їхніх значень [За межами джерел].
2.  **Інтелектуальні обмеження (Smart Constraints):** AI може аналізувати бізнес-логіку та автоматично додавати умови (constraints), щоб виключати неможливі комбінації параметрів ще на етапі створення моделі [За межами джерел].
3.  **Автоматичний переклад у скрипти:** Gemini може брати згенеровані PICT комбінації та миттєво перетворювати їх на готові автотести (наприклад, Selenium або PyTest) [За межами джерел].

### Сценарій сервісу (PICT + ID_{$} + Gemini)

Створення сервісу **"Instant QA Plan"** на вашому сайті.

**Алгоритм роботи:**
1.  **Ввід (ID_{$}):** Користувач на вашому сайті вставляє опис нової фічі (наприклад, "Форма реєстрації з полями: email, пароль, країна").
2.  **Аналіз (Gemini):** Ваші Python-скрипти `ID_{$}` передають цей текст до Gemini. Модель виділяє параметри та можливі значення, формуючи структуру для PICT.
3.  **Обробка (PICT):** `ID_{$}` запускає `pict.exe` (або використовує `clidll`) з підготовленою моделлю. PICT видає оптимальну таблицю тест-кейсів.
4.  **Фіналізація:** Gemini бере цю таблицю і генерує для користувача детальний план тестування з кроками та очікуваними результатами.
5.  **Результат:** Користувач отримує на сайті готовий набір тестів, який гарантує покриття всіх пар параметрів за лічені секунди.

---

## 📋 План дій для Notion
| Крок | Дія | Результат |
| :--- | :--- | :--- |
| **1** | Збірка проекту через `_build.cmd` для отримання `pict.exe` | Готовий до роботи інструмент |
| **2** | Інтеграція `ID_{$}` (Python) для виклику CLI PICT | Автоматизація комбінаторних розрахунків |
| **3** | Підключення Gemini API для генерації файлів `.model` | "Розумний" вхідний інтерфейс |
| **4** | Налаштування GitHub Actions для CI/CD сервісу | Автоматичне тестування та розгортання |

---

### 💡 Резюме

**Суть:** Генератор оптимальних комбінаторних тест-кейсів.

**AI-Роль:** Автоматичне створення тестових моделей та скриптів.
