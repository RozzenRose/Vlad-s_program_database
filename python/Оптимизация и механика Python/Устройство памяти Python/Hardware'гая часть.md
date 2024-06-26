#python #память #механика


Компьютерная память - это буквально часть вычислительной машины. Поделим ее на два разных типа по принципу участия в вычислительных процессов.
- внутренняя память - озу, vram GPU, кэш CPU, харды
- внешняя память - флешки, внешние харддрайвы, СD, DVD

![[Pasted image 20240521142631.png]]

Кэш-память служит для увеличения производительности компьютера.
#### Внутренняя память

Да там и так все понятно, распишу только про кэш.

Кэш-память используется при обмене данными между процессором и оперативной памятью. Алгоритм ее работы позволяет сократить частоту обращений процессора к оперативной памяти и, следовательно, повысить производтельность копьютера.

![[Pasted image 20240521143745.png]]

Современные CPU оснащены кэшем, который состоит зачастую из трех уровней: L1, L2, L3.

**Кэш первого уровня (L1)** - наиболее быстрый уровень кэш-памяти, который работает напрямую с ядром процессора, благодаря этому плотному взаимодействию, данный уровень обладает наименьшим временем доступа и работает на частотах, близких процессору. Размер данного кэша обычно не велик, четырехъядерный процессор Intel Core i7-3770K оснащен 4×32Кб=128Кб кэш-памяти первого уровня (на каждое ядро по 32Кб).

**Кэш второго уровня (L2)** – второй уровень более масштабный, нежели первый, но в результате, обладает меньшими скоростными характеристиками. Четырехъядерный процессор Intel Core i7-3770K оснащен 4×256КБ=1Мб кэш-памяти второго уровня (на каждое ядро по 256Кб).

**Кэш третьего уровня (L3)** – третий уровень, более медленный, нежели два предыдущих. Но всё равно он гораздо быстрее, чем оперативная память. Объём кэша L3 в i7-3770K составляет 88 Мбайт. Если два предыдущих уровня разделяются на каждое ядро, то данный уровень является общим для всего процессора. Показатель довольно солидный, но не заоблачный.
#### Внешняя память
Тут тоже все понятно и так



### Управление памятью
Процесс выделения, распределения и координации памяти таким образом, чтобы все программы работали правильно и могли оптимально получать доступ к различным системным ресурсам. Управление памятью также включает в себя и очистку памяти от объектов, которые больше не нужны.

Существует два типа управления памятью:
- ручное управление памятью
- автоматическое управление памятью

Ручное управление памятью включает в себя, как правил, три этапа:
1. запрос памяти у операционной системы
2. работа с ней
3. возвращение памяти обратно в операционную систему

Ручной подход управления позволяет работать с памятью максимально эффективно. Мы точно знаем, сколько памяти нам выделено, зачем мы ее используем и т.д. Однако помимо преимуществ такой подход имеет и ряд недостатков. Ключевой из них - сложность. Управлять памятью вручную - сложно и тяжело, поскольку легко забыть вернуть память обратно операционной системе, в результате чего возникает утечка: программа держит неиспользуемую память просто так, не давая применять ее для решения других задач.

Автоматическое управление памятью берет на себя самый сложный этап - возвращение памяти обратно операционной системе, когда она уже не требуется. Восстановленная память может использоваться другими объектами. Это пусть и немного менее эффективно, но позволяет сильно сократить трудозатраты на управление памятью и повысить недежность этого процесса.

К языкам с ручным управлением памятью относятся: C, C++, Pascal и многие другие.

К языкам с автоматическим управлением памяти относятся: Python, C#, Java, Ruby, JavaScript и многие другие.

