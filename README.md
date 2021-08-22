#### Оглавление

1. [Unlock (Русский)](#Инструкция-по-разблокировке-максчастоты-на-все-ядра-а-не-на-два-unlock), [Unlock (English)](#Instructions-for-unlocking-the-maximum-frequency-for-all-cores-not-two-unlock), также доступна [Видео-инструкция](#Видео-инструкция-спасибо-Zerg_fb)
2. [Undervolting (Русский)](#Подбор-оптимальных-значений-смещения-напряжений-на-вашем-процессоре-undervolting), [Undervolting (English)](#Finding-the-optimal-voltage-offset-values-for-your-processor-undervolting)
3. [Unlock и Undervolting с помощью Ultimate Patcher Tool](#Ultimate-Patcher-Tool)
4. [Настройка таймингов оперативной памяти](#Настройка-таймингов-оперативной-памяти)
5. [Отключение бипера](#Отключение-бипера)
6. [Часто задаваемые вопросы](#Часто-задаваемые-вопросы)
7. [О пост-кодах](#О-пост-кодах)
8. [Если вы наблюдаете частоту системной шины меньшую, чем 99,75МГц](#Если-вы-наблюдаете-частоту-системной-шины-меньшую-чем-9975МГц)
9. [Настройка управления оборотами вентиляторов для Huananzhi X99-8M/8MD3/F8/T8/TF](#Настройка-управления-оборотами-вентиляторов-для-Huananzhi-X99-8M8MD3F8T8TF)
10. [Если у вас повреждён сокет](#Если-у-вас-повреждён-сокет)

#### Инструкция по разблокировке макс.частоты на все ядра, а не на два (unlock)

Перед началом убедитесь, что используете [актуальную версию S3TurboTool](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/S3TurboTool_v1.53cat_S3THv1_DXETHv1_RAWTHv1b.rar)

*(за данную утилиту и новый драйвер анлока особая благодарность ***ser8989***)*

1. Запускаем S3TurboTool и нажимаем MMTool5
2. В появившейся утилите "MMTool Aptio" нажимаем "Load Image" и выбираем необходимый биос
3. Переходим на вкладку "CPU Patch", выделяем микрокод "6F 06F2", отмечаем "Delete a Patch Data", нажимаем Apply и соглашаемся
4. Нажимаем "Save Image" и закрываем "MMTool Aptio"
5. В S3TurboTool нажимаем AMIBCP
6. В появившейся утилите AMIBCP открываем биос
7. Раскрываем список и идём по пути "Common RefCode Configuration > IntelRCSetup > Advanced Power Management Configuration > CPU C State Control" (путь может отличаться на брендовых материнских платах)
8. Справа, в столбце Optimal, двойным кликом меняем значение параметров:
    * "Package C State limit" на "C2 state"
    * "CPU C3 report" на "Enable"
    * "CPU C6 report" на "Disable"
9. Закрываем окно AMIBCP и соглашаемся на сохранение внесённых изменений
10. Если система двухпроцессорная, то переходим к созданию DXE драйвера. Для однопроцессорных систем нужно проверить, есть ли в нашем биосе модуль PchS3Peim. В S3TurboTool нажимаем UEFITool.
11. В появившейся утилите UEFITool открываем биос
12. Раскрываем список и идём по пути "Intel image > BIOS region > 8C8CE578-...(самый нижний, в котором PEI драйверы) >"
13. Примерно среди первых 20 значений ищем 271DD6F2-... (модуль PchS3Peim). Если такой есть, значит будем собирать PEI драйвер. Если такого нет, значит соберём DXE драйвер.

***PEI драйвер*** (поддерживает режим сна, поддерживает только однопроцессорные системы):

*ВНИМАНИЕ! На некоторых брендовых материнских платах есть вероятность неработоспособности биоса с данным драйвером, в таком случае рекомендуется собрать RAW драйвер.*
1. В S3TurboTool нажимаем "Собрать драйвер"
2. Настраиваем необходимые смещения напряжения ([методика нахождения примерных значений](#Подбор-оптимальных-значений-смещения-напряжений-на-вашем-процессоре-undervolting)). Также выбираем, нужен ли дополнительный сигнал при включении и выводе системы из сна. Нажимаем "Собрать драйвер".
3. В S3TurboTool нажимаем UEFITool
4. В появившейся утилите UEFITool открываем биос
5. Раскрываем список и идём по пути "Intel image > BIOS region > 8C8CE578-...(самый нижний, в котором PEI драйверы) >"
6. Находим 271DD6F2-... (модуль PchS3Peim)
7. Правый клик по нему, нажимаем "Replace as is...", выбираем собранный ранее драйвер (находится в папке S3TurboHack)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen01.png)

8. Выбираем "File > Save image file...", сохраняем. Биос готов к прошивке. Прошить можно также соответствующей кнопкой в S3TurboTool. Если при этом возникает ошибка 103, то следует перезагрузить систему.

***DXE драйвер*** (не поддерживает режим сна, поддерживает одно- и двухпроцессорные системы):
1. В S3TurboTool нажимаем "Собрать драйвер"
2. Нажимаем в верхнем правом углу кнопку DXE
3. Настраиваем необходимые смещения напряжения ([методика нахождения примерных значений](#Подбор-оптимальных-значений-смещения-напряжений-на-вашем-процессоре-undervolting)). Также выбираем, нужен ли дополнительный сигнал при включении. Нажимаем "Собрать драйвер".
4. В S3TurboTool нажимаем UEFITool
5. В появившейся утилите UEFITool открываем биос
6. Раскрываем список и идём по пути "Intel image > BIOS region > 8C8CE578-...(предпоследний, второй снизу, в котором DXE драйверы) >"
7. Прокручиваем в самый низ и находим последний DXE драйвер в списке
8. Правый клик по нему, нажимаем "Insert after...", выбираем собранный ранее драйвер (находится в папке DXETurboHack)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen02.png)

9. Выбираем "File > Save image file...", сохраняем. Биос готов к прошивке. Прошить можно также соответствующей кнопкой в S3TurboTool. Если при этом возникает ошибка 103, то следует перезагрузить систему.

***RAW драйвер*** (поддерживает режим сна, поддерживает только однопроцессорные системы):

*ВНИМАНИЕ! После установки RAW драйвера не редактируйте этот биос любыми программами, в противном случае биос станет неработоспособным.*
1. В S3TurboTool нажимаем "Собрать драйвер"
2. Нажимаем в верхнем правом углу кнопку RAW и выбираем необходимый биос
3. Настраиваем необходимые смещения напряжения. Нажимаем "Собрать и установить драйвер".
4. Биос сохранился в ту же папку и готов к прошивке. Прошить можно также соответствующей кнопкой в S3TurboTool. Если при этом возникает ошибка 103, то следует перезагрузить систему.

Если имеются какие-то проблемы после прошивки, то джампером сбрасываем настройки CMOS

При возникновении трудностей, а также если у Вас есть замечания и пожелания, обращайтесь в [Telegram группу](https://t.me/chinese_lga2011_3_x99) или в эту [Telegram группу](https://t.me/RuSupportHW), если вопрос общекомпьютерный, не тематический.

#### Видео-инструкция *(спасибо Zerg_fb)*:

[![](https://i.ytimg.com/vi/2hfhdrIrXR4/mqdefault.jpg)](https://youtu.be/2hfhdrIrXR4)

#### Instructions for unlocking the maximum frequency for all cores, not two (unlock)

Make sure you are using the [latest version of S3TurboTool](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/S3TurboTool_v1.53cat_S3THv1_DXETHv1_RAWTHv1b.rar) before starting.

*(special thanks to ***ser8989*** for this utility and the new unlock driver)*

1. Launch S3TurboTool and click MMTool5
2. In the appeared utility "MMTool Aptio", click "Load Image" and select the required BIOS
3. Go to the "CPU Patch" tab, select the "6F 06F2" microcode, mark "Delete a Patch Data", click Apply and agree
4. Click "Save Image" and close "MMTool Aptio"
5. In S3TurboTool, click AMIBCP
6. In the appeared utility AMIBCP, open the BIOS
7. Expand the list and follow the path "Common RefCode Configuration > IntelRCSetup > Advanced Power Management Configuration > CPU C State Control" (the path may differ on branded motherboards)
8. On the right, in the Optimal column, double-click to change the value of the parameters:
    * "Package C State limit" to "C2 state"
    * "CPU C3 report" to "Enable"
    * "CPU C6 report" to "Disable"
9. Close the AMIBCP window and agree to save the changes made
10. If the system is dual-processor, then proceed to creating the DXE driver. For uniprocessor systems, you need to check if there is a PchS3Peim module in our BIOS. In S3TurboTool, click UEFITool.
11. In the UEFITool utility that appears, open the BIOS
12. We open the list and follow the path "Intel image > BIOS region > 8C8CE578-...(the lowest one, in which the PEI drivers) >"
13. We are looking for 271DD6F2-... (PchS3Peim module) among the first 20 values. If there is one, then we will build the PEI driver. If this is not the case, then we will build the DXE driver.

***PEI driver*** (supports sleep mode, only supports uniprocessor systems):

*ATTENTION! On some branded motherboards there is a possibility that the BIOS will not work with this driver, in which case it is recommended to build a RAW driver.*
1. In S3TurboTool, click "Собрать драйвер"
2. We adjust the required voltage offsets ([method for finding approximate values](#Finding-the-optimal-voltage-offset-values-for-your-processor-undervolting)). We also choose whether an additional signal is needed when turning on and waking the system from sleep. Click "Собрать драйвер".
3. In S3TurboTool, click UEFITool
4. In the UEFITool utility that appears, open the BIOS
5. We open the list and follow the path "Intel image > BIOS region > 8C8CE578-...(the lowest one, in which the PEI drivers) >"
6. Find 271DD6F2-... (PchS3Peim module)
7. Right click on it, click "Replace as is...", select the previously assembled driver (located in the S3TurboHack folder)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen01.png)

8. Choose "File > Save image file...", save. BIOS is ready for flashing. You can also flash it with the corresponding button in S3TurboTool. If this causes error 103, you must reboot the system.

***DXE driver*** (does not support sleep mode, supports single and dual processor systems):
1. In S3TurboTool, click "Собрать драйвер"
2. Press the DXE button in the upper right corner
3. We adjust the required voltage offsets ([method for finding approximate values](#Finding-the-optimal-voltage-offset-values-for-your-processor-undervolting)). We also choose whether an additional signal is needed when turning on. Click "Собрать драйвер".
4. In S3TurboTool, click UEFITool
5. In the UEFITool utility that appears, open the BIOS
6. Expand the list and follow the path "Intel image > BIOS region > 8C8CE578-...(penultimate, second from the bottom, in which DXE drivers) >"
7. Scroll to the bottom and find the last DXE driver in the list
8. Right click on it, click "Insert after...", select the previously assembled driver (located in the DXETurboHack folder)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen02.png)

9. Choose "File > Save image file...", save. BIOS is ready for flashing. You can also flash it with the corresponding button in S3TurboTool. If this causes error 103, you must reboot the system.

***RAW driver*** (supports sleep mode, only supports uniprocessor systems):

*ATTENTION! After installing the RAW driver, do not edit this BIOS with any programs, otherwise the BIOS will become inoperable.*
1. In S3TurboTool, click "Build Driver"
2. Press the RAW button in the upper right corner and select the required BIOS
3. Adjust the required voltage offsets. Click "Собрать и установить драйвер".
4. The BIOS is saved in the same folder and is ready for flashing. You can also flash it with the corresponding button in S3TurboTool. If this causes error 103, you must reboot the system.

If problems arise after the firmware, use the jumper to reset the CMOS settings

If you have any difficulties, as well as if you have comments and suggestions, please contact the [Telegram group](https://t.me/chinese_lga2011_3_x99)

#### Подбор оптимальных значений смещения напряжений на вашем процессоре (undervolting)
---
Как известно, Китай платы не позволяют адекватно, в привычном виде, менять напряжение процессора для 26хх v3 (46xx v3) систем. При использовании анлока турбобуста используется снижение напряжения, чтобы процессор работал дольше или на большей частоте. Но снижение напряжения офсетом происходит одновременно во всех режимах (их больше трёх). И самая проблемная часть - напряжение в простое. Таким образом, при подборе драйвера анлока с цифрами, к примеру "V3_MOF_705050.fss" происходит снижение напряжения питания процессора на 0.07V во всех режимах работы, снижение кэша процессора на 0.05V во всех режимах работы. Опыт показал, что нестабильность, в подавляющем большинстве случаев, проявляется в состоянии простоя системы (бездействия). Таким образом, можно без использования анлока, в штатном режиме, снижать частоту процессора офсетом и проводить тестирование, без боязни прошивки биоса и подобных сопутствующих проблем. А уже в дальнейшем, когда будут известны рабочие напряжения простоя процессора, снижать их раз и навсегда биосом с помощью программы (S3TurboTool  Sergey Bakaev ser8989). Изменение будет происходить с помощью софта и при зависании системы или перезагрузке все вернётся в исходное состояние.

Подойдут несколько программ для изменения напряжения. Intel XTU, QuickCPU и ThrottleStop. В общем виде инструкция такая: снижаем напряжение процессора, тестируем разными программами в нагрузке, если хорошо, то оставляем на длительное использование. Обычно рабочим снижением будет считаться от 40 до 90, в зависимости от модели процессора и конкретного экземпляра. (для L процессоров может быть рабочим и 120).

Вторым шагом снижаем напряжение на кэше процессора (Cache). Обычно в драйверах выбирается значение 50-60, но на практике оно легко может превышать и 120. Нестабильность проверять программой LinX с AVX-инструкциями и программами, сильно нагружающими память (к примеру TestMem5). Снижение третьего напряжения - SystemAgent, мало изучено, но стандартное снижение 0 или 50.

После нахождения значений, при которых система стабильно работает как в простое, так и в нагрузке, можно данные значения прошивать в биос материнской платы.

*(автор ***Василий Пупкин***)*

---

У каждой модели процессора есть значение TDP, это максимальная его мощность в ваттах. В некоторых задачах происходит достижение этого предела, и процессор начинает уменьшать частоту на ядрах шагами по 100МГц, чтобы не перейти за эту грань. Мощность, это напряжение, умноженное на ток. Понижая напряжение, достигается улучшение энергоэффективности процессора, т.е. при той же нагрузке снижается потребление энергии и как следствие тепловыделение. Соответственно, при той же граничной нагрузке, процессор сможет более уверенно удерживать частоту.

Затронем два блока в наших процессорах - Core (ядра) и Cache (кэш) (есть ещё блок SystemAgent, но снижение на нём напряжения, не даёт снижения потребления процессора, поэтому лучше не менять его).

Напряжение понижается методом его смещения (offset) на определённую величину. Т.е. во всех режимах работы процессора напряжение будет меньше на заданное нами значение. Можно безопасно произвести поиск значений смещения на ядрах на системе без анлока. Можно сделать это и на системе с активным анлоком, в таком случае при изменении смещения, анлок пропадёт. Это никак не повлияет на поиск значения, т.к. у ядер напряжение меняется в зависимости от его частоты.

Для изменения смещения используем программу Intel XTU, QuickCPU или ThrottleStop. Далее будет рассмотрено на примере последней.

Перед дальнейшими действиями, сохраните важные данные в вашей системе, будьте готовы к возможным зависаниям или синему экрану.
1. После запуска [ThrottleStop](https://www.techpowerup.com/download/techpowerup-throttlestop/) нажимаем FIVR
2. В блоке "FIVR Control" отмечено "CPU Core", значит меняем смещение на ядрах
3. В блоке "CPU Core Voltage" выбираем "Unlock Adjustable Voltage", понижаем "Offset Voltage", например, до -100mV и нажимаем Apply. Если система зависла или мы увидели синий экран, значит такое смещение нам точно не подходит, перезагружаем систему и пробуем -95mV и т.д.
4. Если всё вроде бы стабильно, значит запускаем [OCCT](https://www.ocbase.com/), выставляем режим теста "CPU/Набор данных Большой/Режим Тяжёлый/Нагрузка Переменная/Инструкции SSE/Потоки Авто". Почему SSE, а не AVX? Потому что при AVX-нагрузке процессор переходит в другой режим работы, прибавляет напряжение и снижает частоту. Это не даст результата при тестировании стабильности работы ядер. Перед запуском теста заботимся о должном охлаждении процессора, области VRM и оперативной памяти (она может троттлить без дополнительного охлаждения).
5. Запускаем тест. Если система зависла или мы увидели синий экран (обычно ошибка CLOCK_WATCHDOG_TIMEOUT), то уменьшаем наше смещение и продолжаем тестировать до появления стабильности (1 час). Это и будет нашим значением для ядер.

Далее переходим к тесту кэша (начать можно с -125mV). Тестировать кэш оптимально в LinX (проверено на [v0.6.5](https://github.com/sanekgusev/LinX-old/releases/tag/0.6.5)), выставляем настройку 8192МБ памяти, нажимаем Start. При нестабильности возможен синий экран (обычно ошибка WHEA_UNCORRECTABLE_ERROR). Достаточно 5 минут теста.

Т.к. для проверки кэша нужны плотные потоки данных, то в дальнейшем рекомендуется ещё раз провести тест, но уже с анлоком вкупе с багом SVID/FIVR (уделите особое внимание температуре во время теста)

#### Finding the optimal voltage offset values ​​for your processor (undervolting)
---
As you know, China boards do not allow to adequately, in the usual form, change the processor voltage for 26xx v3 (46xx v3) systems. When using a turbo-boost unlock, a voltage reduction is used to make the processor work longer or at a higher frequency. But voltage reduction by offset occurs simultaneously in all modes (there are more than three of them). And the most problematic part is stress in idle time. Thus, when selecting an unlock driver with numbers, for example "V3_MOF_705050.fss", the processor voltage decreases by 0.07V in all operating modes, the processor cache decreases by 0.05V in all operating modes. Experience has shown that instability, in the overwhelming majority of cases, manifests itself in a system idle state (inactivity). Thus, it is possible, without using an unlock, in the normal mode, to reduce the processor frequency by offset and conduct testing without fear of BIOS firmware and similar related problems. And in the future, when the processor idle operating voltages are known, reduce them once and for all by the BIOS using the program (S3TurboTool Sergey Bakaev ser8989). The change will occur with the help of software, and when the system hangs or reboots, everything will return to its original state.

Several programs are suitable for changing the voltage. Intel XTU, QuickCPU and ThrottleStop. In general, the instruction is as follows: we lower the processor voltage, test it with different programs in the load, if it is good, then we leave it for long-term use. Typically, a working degradation will be considered from 40 to 90, depending on the processor model and specific instance. (for L processors, 120 can be working).

The second step is to lower the voltage on the processor cache. Usually the driver chooses a value of 50-60, but in practice it can easily exceed 120. Check the instability with the LinX program with AVX-instructions and programs that heavily load memory (for example TestMem5). Reduction of the third voltage - SystemAgent, little studied, but the standard decrease is 0 or 50.

After finding the values ​​at which the system works stably both in idle and in load, these values ​​can be flashed into the BIOS of the motherboard.

*(by ***Vasily Pupkin***)*

---

Each processor model has a TDP value, this is its maximum power in watts. In some tasks, this limit is reached, and the processor begins to reduce the frequency on the cores in steps of 100MHz in order not to go beyond this limit. Power is voltage times current. Lowering the voltage improves the processor's energy efficiency, i.e. at the same load, energy consumption and, as a consequence, heat generation are reduced. Accordingly, with the same boundary load, the processor will be able to hold the frequency more confidently.

Let's touch on two blocks in our processors - Core and Cache (there is also a SystemAgent block, but lowering the voltage on it does not reduce processor consumption, so it is better not to change it).

The voltage is lowered by offsetting it by a certain amount. That is, in all operating modes of the processor, the voltage will be less by the value set by us. It is safe to search for bias values ​​on cores on a system without unlock. You can do this on a system with an active unlock, in which case when the offset is changed, the unlock will disappear. This will not affect the search for a value in any way. in nuclei, the voltage changes depending on its frequency.

To change the offset, we use the Intel XTU, QuickCPU or ThrottleStop program. Further it will be considered on the example of the latter.

Before proceeding, save important data on your system, be prepared for possible freezes or blue screens.
1. After launching [ThrottleStop](https://www.techpowerup.com/download/techpowerup-throttlestop/), press FIVR
2. In the "FIVR Control" block, "CPU Core" is marked, which means we change the offset on the cores
3. In the "CPU Core Voltage" block, select "Unlock Adjustable Voltage", lower the "Offset Voltage", for example, to -100mV and click Apply. If the system freezes or we see a blue screen, then such an offset is definitely not suitable for us, we reboot the system and try -95mV, etc.
4. If everything seems to be stable, then we launch the [OCCT](https://www.ocbase.com/), set the test mode "CPU/Data set Large/Mode Extreme/Load type Variable/Instruction set SSE/Threads Auto". Why SSE and not AVX? Because under AVX load, the processor switches to another operating mode, adds voltage and decreases frequency. This will not work when testing the stability of the cores. Before running the test, we take care of proper cooling of the processor, VRM area and RAM (it can throttle without additional cooling).
5. We run the test. If the system freezes or we see a blue screen (usually the CLOCK_WATCHDOG_TIMEOUT error), then we reduce our offset and continue testing until stability appears (1 hour). This will be our core value.

Next, we move on to the cache test (you can start with -125mV). Optimally test the cache in LinX (tested on [v0.6.5](https://github.com/sanekgusev/LinX-old/releases/tag/0.6.5)), set the 8192MB memory setting, press Start. In case of instability, a blue screen is possible (usually a WHEA_UNCORRECTABLE_ERROR error). 5 minutes of the test is enough.

Because to check the cache, dense data streams are needed, then it is recommended to run the test again in the future, but this time with unlock, coupled with the SVID/FIVR bug (pay special attention to the temperature during the test)

#### Ultimate Patcher Tool

В апреле 2021 года в свет вышла программа [Ultimate Patcher Tool](https://nalex.me/ultimate-patcher-tool/) (далее UPT) для более удобного процесса модификации биоса с целью анлока и андервольта и даже для замены логотипа. Также на некоторых биосах возможно устранение проблемы, из-за которой смартфан не работал после возвращения системы из режима сна.

UPT поддерживает загрузку нужного биоса напрямую с гитхабов, также предлагает выбрать, что именно нужно модифицировать. Если не удаётся выбрать исправление смартфана, значит это пока не поддерживается для выбранного биоса. Поддерживает не только C612/X99 чипсеты, а также и другие, которые устанавливаются на платформу LGA2011-3.

После модификации, биос нужно прошить любым способом (программатор, afuwin, fptw64, [S3TurboTool](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/S3TurboTool_v1.53cat_S3THv1_DXETHv1_RAWTHv1b.rar))

*ВНИМАНИЕ! На данный момент проект в стадии тестирования, поэтому не прошивайте модифицированный биос, если у вас нет возможности восстановиться с помощью программатора.*

В настройках BIOS, обычно в разделе IntelRCSetup, должно быть меню "OverClocking Feature", в котором будут следующие меню:
- Processor > Core Voltage Offset (смещение напряжения ядер в мВ, обращаем внимание на его префикс)
- CLR/Ring > CLR Voltage Offset (смещение напряжения кэша в мВ, обращаем внимание на его префикс)
- Uncore > Uncore Voltage Offset (смещение напряжения SystemAgent в мВ, обращаем внимание на его префикс)
- SVID/FIVR > SVID Support (отключение равнозначно включению "Баг SVID/FIVR" в S3TT)

Проверенные биосы:
- Huananzhi X99-8M/8MD3/F8/T8/TF (25.05.2020) всё работает
- Huananzhi X99-8M/8MD3/F8/T8/TF (17.08.2020 и новее) !НЕ РАБОТАЕТ! - пост-код 79
- Huananzhi X99-F8D/T8D (24.06.2020) всё работает, кроме смартфана
- Jginyue X99M-PLUS D3 (03.11.2020) всё работает, кроме смартфана

Список будет пополняться, сообщайте о результатах в [Telegram группу](https://t.me/chinese_lga2011_3_x99)

#### Настройка таймингов оперативной памяти
1. преисполниться какнада
2. открыть в aida64 вкладку SPD, сделать скриншот, скинуть в избранное себе в телеге
3. открыть там же вкладку на 1 ниже, скрин, себе
4. снизить на 1 первые 3 тайминга, 4ый снизить на 2, тест кэша и памяти в аиде, запомнить результаты и значения таймингов
5. повторять пока загружается, при нестарте повысить обратно первые 3 и снижать по 1 только 4ый
6. промежуточное преисполнение, переходим к трфс
7. смотрим там же в SPD из п2 минимальное значение tRFC, прибавляем к нему сколько то до числа кратного 8, при нестарте еще 8
8. к минимальному tRFC на ктр запустится прибавляем еще 8-16 и тестим так же в аиде кэши и память
9. устанавливаем параметр tREFI в 32767 если ваши осн тайминги (первые 3) нечетные, и 32760 если четные
10. хорошо тестим в аиде
11. тестим в мемтесте со стандартным профилем настроек
12. тестим стабильность системы и на предмет вылетов в играх
13. какнада, максимальное преисполнение

*(автор ***KOT JIETA***)*

**Быстрый тест на стабильность.**

**Если после модификации таймингов система не стартует, то сбрасываем CMOS перемычкой.**

**Если старт прошел успешно, то проверяем объём памяти. Если он сократился, то данная конфигурация нестабильна. Если всё в порядке, то запускаем [TestMem5](https://testmem.tz.ru/tm5.rar) и ждём его завершения (обычно около 5-10 минут).**

1. Запускаем в [AIDA64](https://www.aida64.com/downloads) тест кэша и памяти, в окне теста делаем двойной клик по надписи Memory, дожидаемся завершения, и записываем результаты. Также запускаем [LinX](https://github.com/sanekgusev/LinX-old/releases/tag/0.6.5) и делаем один проход с настройкой 8192МБ, записываем значение GFlops.
2. Создаём табличку:
* Частота
* tREF
* CL
* tRP
* tRCD
* tRAS
* tWR
* tRFC
* tRRD
* tRTP
* tWTR
* tFAW
* tRC
* tCWL

В неё запишем все текущие тайминги на текущей частоте. Переходим в AIDA64 > Системная плата > Чипсет. Переписываем тайминги оттуда. tRC там не будет, а tCWL это tWCL. У tRRD и tWTR записываем значение "Diff. Bank Group".
3. Далее переходим в настройки биос и меняем частоту памяти на другую, например с 2133 меняем на 1866. И проделываем тоже самое, добавляем реальные тайминги в табличку. Также и с частотами 1600 и 1333.
4. Теперь ставим нашу необходимую частоту, и переходим в раздел управления таймингами. Меняем Command Timing на 1N и тестируем на стабильность. Если тест не пройден успешно, то меняем на 2N и т.д.
5. Снижаем три тайминга одновременно CAS Latency (CL), tRP и tRCD на единицу и тестируем (смотрим в нашу табличку, там указаны исходные тайминги). Если стабильно, то снижаем ещё на единицу и т.д. до выявления минимального стабильного значения.
6. После того, как обнаружен предел, снижаем только CL на единицу, и тестируем, до выявления минимального стабильного значения.
7. Снижаем tRFC и тестируем. Скорее всего примерным значением будет то, которое по табличке рядом с теми же tRP и tRCD что получились у нас.
8. Повышаем tREF и тестируем. Дополнительно тестируем в LinX, т.к. возможна ситуация с падением производительности.
9. Снижаем остальные тайминги до уровня, которые по табличке рядом с теми же tRP и tRCD что получились у нас.
10. По итогу делаем тест кэша и памяти и тест в LinX, и радуемся возросшей производительности. Также результатами можно поделиться в [Telegram группе](https://t.me/chinese_lga2011_3_x99), там же можно задать любой возникший вопрос.

#### Отключение бипера
1. Нам нужны определённые модули из биоса, чтобы их получить открываем [S3TurboTool](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/S3TurboTool_v1.53cat_S3THv1_DXETHv1_RAWTHv1b.rar), нажимаем UEFITool и в появившейся утилите открываем биос
2. Раскрываем список и идём по пути "Intel image > BIOS region > 8C8CE578-...(предпоследний, второй снизу, в котором DXE драйверы) >"
3. В самом правом столбце есть названия модулей, ищем Bds(ищите начиная сверху) и StatusCodeDxe(ищите начиная снизу)
4. Правый клик по "PE32 image section", нажимаем "Extract body...", сохраняем модуль с соответствующим названием, чтобы не запутаться. Также делаем и со вторым модулем.

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen03.png)

5. В S3TurboTool нажимаем Beep в верхнем правом углу
6. Нажимаем левый значок для открытия файла модуля и выбираем один из модулей
7. Нажимаем правый значок "Beep Off" и делаем то же самое со вторым модулем
8. Теперь снова открываем UEFITool (повторяем шаги 1-3 инструкции), делаем правый клик по "PE32 image section", нажимаем "Replace body...", и выбираем соответствующий модуль. Также делаем и со вторым модулем.
9. Выбираем "File > Save image file...", сохраняем. Биос готов к прошивке. Прошить можно также соответствующей кнопкой в S3TurboTool.

*Если бипер по-прежнему издаёт звуки, то попробуйте также обработать модуль StatusCodePei (в разделе с PEI модулями). Если и это не поможет, то пожалуйста сообщите об этом в [Telegram группу](https://t.me/chinese_lga2011_3_x99).*

#### Часто задаваемые вопросы
---
***Вопрос:*** В чём преимущество нового драйвера от S3TurboTool перед драйверами от payne и MOF?

***Ответ:*** Была решена проблема, при которой анлок сбрасывался при выводе системы из режима сна. Также было уменьшено энергопотребление процессора без нагрузки.

---
***Вопрос:*** Кот, расскажи, какие ещё есть возможности у нового драйвера от S3TurboTool?

***Ответ:*** Можно снизить напряжение на процессоре не используя анлок, для этого не выполняем шаги 1-9 инструкции и на этапе сборки драйвера снимаем галку "Разблокировка турбо". Также есть опция "Баг SVID/FIVR", это исказит отображение реального потребления процессора, в связи с чем в AVX-нагрузке перестанет падать частота, но соответственно вырастет потребление. Рекомендуется для моделей, у которых низкий заводской TDP, например E5-2630Lv3.

---
***Вопрос:*** Как поддержать?

***Ответ:*** Все прорывные исследования и реализацию новых возможностей производит ser8989 - 5366-7280-9045-9598 (МТС Банк)

Кошак ^-^ - Qiwi +7-902-два13-33-10 или 4276-6900-1026-7273 (Сбербанк)

KOT JIETA - 5228-6005-4997-9898 (Сбербанк)

Василий Пупкин - 5368-2902-1226-6714 (Банк ВТБ)

Куратор [темы на Overclockers](https://forums.overclockers.ru/viewtopic.php?f=1&t=602922&view=unread#unread) Вадим v111 - 5167-4901-9610-2512 (Украина)

Костян - 4276-0200-1999-5122 (Сбербанк)

Всем большое спасибо за помощь!

---

#### О пост-кодах

Известные решения при определённых пост-кодах:

* FF, 19 - требуется прошивка биос (если система после запуска сразу выключается, успев показать только FF, то это значит что система не может запустить БП, либо БП сразу уходит в защиту по какой-то причине)
* CD - установлен V4 ES (они несовместимы)
* B7 - пробовать сброс настроек биоса перемычкой

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen06.png)
![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen07.png)
![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen08.png)

#### Если вы наблюдаете частоту системной шины меньшую, чем 99,75МГц
Например, 98МГц. В таком случае вы не получите 100МГц, даже если прошьёте соответствующий биос. Насколько известно на данное время, частоту занижает активный Hyper-V в ОС.

*(спасибо ***Василию Пупкину***)*

#### Настройка управления оборотами вентиляторов для Huananzhi X99-8M/8MD3/F8/T8/TF

Представляет собой настраиваемую кривую зависимости оборотов от температуры процессора. Настраиваются пять точек этой кривой. Т1 определяет температуру, до достижения которой будут обороты, определённые в PWM1 (значение в %). T5 определяет температуру, после достижения которой будут 100% обороты. T2/T3/T4 являются промежуточными точками, с помощью которых возможно построить кривую между T1 и T5.

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen04.png)

На графике выше представлен вариант из kot-версий биоса. До 45° поддерживаются минимальные обороты (30%) и далее линейно растут до 100% при достижении 80°.

Для лучшего понимания, рассмотрим другой вариант настройки.

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen05.png)

Что такое Tbase0? Это настраиваемое смещение выставленных значений температуры. При значении 0 значения температуры совпадают с температурой CPU Package (DTS). В версии биоса 2020-08-17 нет настройки Tbase0, но это смещение имеет значение 15, именно поэтому все значения температур в настройке занижены на 15, чтобы соответствовать желаемому.

Также биос от 2020-08-17 позволяет настраивать % оборотов 4pin-вентилятора, подключённого к разъёму CPU_FAN2 (X99-T8/TF). Обороты статичны и не будут зависеть от температуры. Настройка в пределах значений 0-255.
* 0% - 0
* 10% - 25
* 20% - 51
* 30% - 76
* 40% - 102
* 50% - 127
* 60% - 153
* 70% - 178
* 80% - 204
* 90% - 229
* 100% - 255

Этот режим также поддерживается и для CPU_FAN1. Удобно изучить возможности своего вентилятора перед его настройкой.

Также имеется проблема, из-за которой нарушается работа управления оборотами после выхода системы из режима сна. Обороты CPU_FAN1 фиксируются на значении 50%, а обороты CPU_FAN2 (X99-T8/TF) на значении 100%. В программе [Ultimate Patcher Tool](#Ultimate-Patcher-Tool) есть исправление этой проблемы для биоса 2020-05-25.

#### Если у вас повреждён сокет

Аккуратно править ножки сокета, например, зубочисткой или подобным предметом.

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen09.png)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen10.png)

На изображениях выше отмечены ножки, отсутствие двух-трёх которых некритично, т.к. ножки многократно продублированы. Если у вас необратимо повреждены другие ножки, тоже может быть шанс на успешную работу системы, для этого необходимо свериться с распиновкой или написать в [Telegram группу](https://t.me/chinese_lga2011_3_x99).
