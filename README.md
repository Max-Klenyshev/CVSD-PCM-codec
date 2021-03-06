# CVSD-PCM-codec
    Код для ВКРб "Разработка на языке Verilog PCM-CVSD кодека с возможностью сжатия по  альфа-закону". 
    Разработка производилась согласно спецификации Bluetooth за 2004 год.
# Описание модулей
## Сжатие по альфа-закону
        Компандирование (от англ. companding — compression и expanding) или нелинейная компрессия – 
    метод обработки сигнала, применяющийся для передачи аудиоданных по каналам с ограниченным
    динамическим диапазонам. Данный метод основывается на том факте, что при увеличении громкости
    можно уменьшать количество уровней квантования сигнала в области высокой громкости, сохраняя
    качество звука. Одним из основных законов аудиокомпандирования является альфа-закон, формула
    которого содержит логарифм. Так как логарифм является несинтезируемой конструкцией в Verilog,
    то в данном пректе был использован дискретный метод кодирования (модули PCM_to_Alaw и 
    Alaw_to_PCM), схожий с кодированием чисел с плавающей точкой. Алгоритм данного метода можно
    найти на [здесь](https://wiki2.org/ru/G.711).
## CVSD
        Работа CVSD-кодека основана на сравнении входного 16-битного звукового сигнала с опорным значением,
    в роли которого выступает сигнал с декодера из CVSD в PCM.
        Декодер работает следующим образом: 
        - бит CVSD передаётся на устройство контроля размера шага квантования (которое не допускает 
    резкого увеличения шага квантования при быстром изменении сигнала), в котором определяется
    необходимость уменьшения или увеличения размера шага квантования (модули shift_reg и comporator)
    а также производятся данные действия (mux)
        - шаг квантования и бит CVSD передаются на аккумулятор (accumulator), где полученное ранее 
    значение срезается, чтобы оно оставалось в рамках, указанных в спецификации
        Указанные ранее модули были объединенины в модуль decoder.
        Энкодер (coder) вычитает из входного PCM-сигнала опорное значение с декодера. 
    Первый бит полученной разности является битом CVSD.
## Интерфейс для микросхемы голосового кодека
        Модуль meandr является интерфейсом для микросхемы голосового кодека W681310 Nuvoton,который 
    использовался в лабораторном стенде, на котором был протестирован аудиокодек.
## Общий модуль
        Все описанные ранее модули были объединены в модуле CORE. Для тестирования имеются петли на 
    интерфейсе кодека голосового канала и на CVSD-кодеке для отдельного тестирования данных модулей.
