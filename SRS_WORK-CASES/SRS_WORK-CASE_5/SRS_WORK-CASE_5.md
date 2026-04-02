# Work-case 5

**Виконав: студент групи РПЗ-33, Руденко Дмитро**

<div align="justify"> 

#### 1. При роботі з персональним комп’ютером дуже часто виникає необхідність підключати периферійне обладнання. На прикладі принтера та флешки опишіть який механізм має ОС Linux для роботи з ними.

Головне правило ОС Linux звучить так: «Все є файлом». Це означає, що пристрої (апаратне забезпечення) представлені в системі у вигляді спеціальних файлів пристроїв, які зазвичай знаходяться у каталозі /dev/.  
Коли ви вставляєте флешку, ядро Linux розпізнає її за допомогою підсистеми udev. У каталозі /dev/ створюється файл блочного пристрою (наприклад, /dev/sdb, де розділ на ній буде /dev/sdb1). Щоб працювати з даними на флешці, цей пристрій потрібно «змонтувати» у
файлову систему.   
При підключенні принтера udev також створює символьний файл пристрою (часто /dev/usb/lp0). Проте безпосередньо в цей файл зазвичай не пишуть. Для керування принтерами в Linux використовується сервер друку CUPS (Common UNIX Printing System). 
Він обробляє черги друку, використовує драйвери (PPD файли) і перетворює документ у формат, зрозумілий принтеру.

_**<span>- В чому суть операції монтування, для чого вона використовується та як?</span>**_

<blockquote>

У Linux немає звичних для Windows "дисків" (C:, D: тощо). Є єдине дерево файлової системи, яке починається з кореня (/). Монтування — це процес "прикріплення" файлової системи пристрою (наприклад, файлів на флешці) до певної існуючої порожньої папки в цьому дереві 
(називається точкою монтування, наприклад, /mnt/usb або /media/user/flashdrive). За допомогою нього ОС та користувач можуть отримати доступ до читання та запису файлів на носії. Без монтування система "бачить", що пристрій підключено фізично, але не має доступу до його 
вмісту. Зазвичай графічне середовище (GNOME, KDE) монтує флешки автоматично при підключенні. Вручну це робиться командою mount.

</blockquote>

_**<span>- В чому різниця при роботі з периферією у ОС Linux та ОС Windows?</span>**_

| Характеристика | ОС Windows | ОС Linux |
| :--- | :--- | :--- |
| Організація дисків | Використовує літери дисків (A:, C:, D:) для кожного окремого розділу чи пристрою | Використовує єдине дерево каталогів (/). Пристрої монтуються в папки (наприклад, /media/usb) |
| Представлення пристроїв | Приховані у диспетчері пристроїв | «Все є файлом» — доступні як файли у каталозі /dev/ |
| Драйвери | Часто вимагають ручного встановлення файлів .exe/.msi від виробника або завантаження через Windows Update | Більшість драйверів (модулів ядра) вже вбудовані в ядро Linux. Пристрої часто працюють "з коробки" |
| Система друку | Власний Windows Print Spooler | Відкритий стандарт CUPS (з веб-інтерфейсом для налаштування на localhost:631) |

#### 2. Підключіть до вашої віртуальної машини зі встановленою ОС Linux флешку та принтер (за можливості) та через графічний інтерфейс скопіюйте один файл з флешки на віртуальну машину та роздрукуйте його (такі ж самі дії повторіть, але з іншим файлом через команди в терміналі).

> **Через графічний інтерфейс (GUI):**

Спочатку підключимо флешку до віртуальної машини:

<img width="883" height="576" alt="1" src="https://github.com/user-attachments/assets/ad5ff2e6-4c20-421e-ba36-a82a2034368c" />

Потім скопіюємо папку з флешки та перенесемо її в папку "Документи" всередині ВМ:

<img width="1343" height="534" alt="2" src="https://github.com/user-attachments/assets/57f497ad-baa6-4754-ad23-a22a4d9f9555" />

Після чого додамо принтер через CUPS (Common UNIX Printing System) - систему друку, яка надає користувачеві зручний спосіб керування друкованими завданнями та налаштуваннями принтера. Для цього відкрию браузер та перейду за адресою [http://localhost:631](http://localhost:631):

<img width="1920" height="955" alt="3" src="https://github.com/user-attachments/assets/ac1f810a-8f8f-42a7-b9ce-32c002c295d3" />

Перейду до вкладки "Administration" та серед інших мережевих принтерів оберу "AppSocket/HP JetDirect". Вибір зумовлений тим, що це «рідна» технологія для принтерів HP, яка гарантує найвищу стабільність роботи у Linux без додаткових мережевих перетворень та дозволяє системі в реальному часі отримувати інформацію про стан друку (помилки, відсутність паперу тощо). Після чого натисну "Continue":

<img width="1088" height="619" alt="4" src="https://github.com/user-attachments/assets/f2bfcd08-6808-45bd-afb0-266c4f37ac0c" />

Тепер потрібно дізнатись API-адресу принтера в мережі. Її можна знайти в налаштуваннях пристроях на хостовій системі або натиснути і потримати кнопку інформації/Wi-Fi на принтері - він роздрукує сторінку конфігурації, де буде вказана його "IPv4 Address". Якщо у принтера є екран, це можна подивитися в налаштуваннях мережі/Wi-Fi. Встановлюємо підключення у такому форматі:

<img width="1137" height="660" alt="5" src="https://github.com/user-attachments/assets/69d06c33-99c4-463a-a92a-a5fd510f0a30" />

Даємо ім'я новому принтеру та тиснемо "Далі":

<img width="1048" height="564" alt="6" src="https://github.com/user-attachments/assets/f9d8a8fc-00eb-426c-9953-d425f8ae426a" />

Обираю виробника, натискаю "Continue":

<img width="1051" height="792" alt="7" src="https://github.com/user-attachments/assets/f47cacd3-289a-4eed-bfd7-3c3a85dfcfa3" />

Переді мною з'явився список доступних моделей принтерів. Обираю свій:

<img width="1049" height="748" alt="8" src="https://github.com/user-attachments/assets/c416e7d7-37b7-48e7-ab71-d4b4117b9bad" />

Бачимо, що принтер успішно додано:

<img width="1074" height="497" alt="9" src="https://github.com/user-attachments/assets/42d23075-036a-4d45-9c14-e023fcf5bfea" />

Далі відкрию файл, оберу принтер та роздрукую його - стандартна процедура:

<img width="1920" height="955" alt="10" src="https://github.com/user-attachments/assets/f49d028d-7b6f-4cf6-9d43-6bfc6d99c13b" />

<img width="1920" height="859" alt="11" src="https://github.com/user-attachments/assets/1d73242d-b37f-4067-9b49-0b4957d39374" />

Отримаю результат:

![12](https://github.com/user-attachments/assets/8197378c-02ab-4b03-a1ab-0236a7685163)

> **Через команди в терміналі (CLI):**

Для початку створю на флешці новий файл (цього разу pdf):

<img width="571" height="192" alt="13_2" src="https://github.com/user-attachments/assets/4b00e89f-738e-4b12-8e66-aab65ae711a3" />

Відкрию термінал та знайду підключену флешку. Переді мною з'явився список пристроїв. Шукаю щось схоже на sdb1 чи sdc1 з розміром моєї флешки:

<img width="820" height="623" alt="13_1" src="https://github.com/user-attachments/assets/6c9b111f-c0e1-4366-9380-c16891e028f3" />

Створю точку монтування (папку):

<img width="622" height="43" alt="13_3" src="https://github.com/user-attachments/assets/5d2babeb-860e-4e61-ba87-6c78c414a631" />

Змонтую флешку та скопіюю інший файл до папки Documents/Work-case-5. Після чого відмонтую флешку, забезпечивши безпечне вилучення:

<img width="807" height="69" alt="13_4" src="https://github.com/user-attachments/assets/9762b1ef-b818-4c74-be0d-55f3df10ec6d" />

Необхідно перевірити, чи бачить система мій принтер і дізнатися його назву. Команда покаже список принтерів і принтер за замовчуванням:

<img width="805" height="111" alt="13_5" src="https://github.com/user-attachments/assets/7561646b-e4cd-425c-b5a7-e149739dc8b0" />

Відправлю файл на друк, використавши утиліту `lp`:

<img width="806" height="57" alt="14" src="https://github.com/user-attachments/assets/61cd23c0-45e0-42d4-94f8-250cb822c46c" />

Отримаю результат:

![15](https://github.com/user-attachments/assets/17895b3a-13f9-40f4-be11-53b82c55f9ad)

#### Словник англійських термінів

| № | Слово | Пояснення |
| :--- | :--- | :--- |
| 1 | **Peripheral device** | Периферійний пристрій - зовнішнє апаратне забезпечення, яке підключається до комп'ютера для розширення його функціоналу |
| 2 | **Mount** | Монтування - процес приєднання файлової системи носія даних до загального дерева каталогів Linux для отримання доступу до файлів |
| 3 | **Root directory** | Кореневий каталог - вершина ієрархії файлової системи в Linux, яка позначається символом слешу |
| 4 | **Command Line Interface (CLI)** | Інтерфейс командного рядка - спосіб взаємодії з операційною системою за допомогою текстових команд у терміналi |
| 5 | **CUPS (Common UNIX Printing System)** | Загальна система друку UNIX - стандартний сервер друку з відкритим вихідним кодом для операційних систем на базі Linux |
| 6 | **AppSocket / HP JetDirect** | Мережевий протокол друку - стандарт прямої передачі даних на мережеві принтери через порт 9100, який забезпечує швидкий друк та двосторонній зв'язок |
| 7 | **Virtual Machine (VM)** | Віртуальна машина - програмне середовище, що імітує роботу фізичного комп'ютера з власною операційною системою |

#### Conclusions:

In this laboratory work, I practically explored the mechanisms of connecting and managing peripheral devices in the Linux operating system. I learned the fundamental concept that Linux treats all hardware as files and requires the mounting process to access storage devices like USB flash drives. During the practical part, I successfully connected a USB drive and an HP printer to the virtual machine, copied a document, and printed it using both the graphical user interface and terminal commands. Furthermore, I gained experience in configuring a network printer via the CUPS server using the optimal AppSocket/HP JetDirect protocol. This practice significantly improved my understanding of the core differences in hardware management between Linux and Windows, as well as enhanced my terminal operation skills.


















