***[> Telegram группа](https://t.me/russian_xeon_community)***

***[> Discord сервер](https://discord.gg/mRNBU5YpzW)***

***[> VK Community X99](https://vk.com/koshak1013)***

***[> VK Community X79](https://vk.com/huanan_x79)***

[Рекомендую ознакомиться с материалом, много полезной информации, поможет верно оценить возможности Xeon X99 и сделать правильный выбор](https://youtu.be/H5xgBq1O77M) *(спасибо PC4fan @ Артур Карпов)*:

[![](https://i.ytimg.com/vi/H5xgBq1O77M/mqdefault.jpg)](https://youtu.be/H5xgBq1O77M)

## Оглавление

1. [Unlock (Русский)](#инструкция-по-разблокировке-максчастоты-на-все-ядра-а-не-на-два-unlock), [Unlock (English)](#instructions-for-unlocking-the-maximum-frequency-for-all-cores-not-two-unlock), также доступна [Видео-инструкция](#видео-инструкция-спасибо-zerg_fb)
2. [Undervolting (Русский)](#подбор-оптимальных-значений-смещения-напряжений-на-вашем-процессоре-undervolting), [Undervolting (English)](#finding-the-optimal-voltage-offset-values-for-your-processor-undervolting)
3. [Unlock и Undervolting с помощью Ultimate Patcher Tool](#ultimate-patcher-tool)
4. [Восстановление биоса программатором](#восстановление-биоса-программатором)
5. [Настройка таймингов оперативной памяти](#настройка-таймингов-оперативной-памяти)
6. [Что делать, если пропадает загрузка системы](#что-делать-если-пропадает-загрузка-системы)
7. [Как открыть скрытые пункты меню настроек биоса](#как-открыть-скрытые-пункты-меню-настроек-биоса)
8. [Добавление поддержки Resizable BAR в биос](#добавление-поддержки-resizable-bar-в-биос)
9. [Отключение бипера](#отключение-бипера)
10. [Поддержка](#поддержка)
11. [О пост-кодах](#о-пост-кодах)
12. [Если вы наблюдаете частоту системной шины меньшую, чем 99,75МГц](#если-вы-наблюдаете-частоту-системной-шины-меньшую-чем-9975мгц)
13. [Настройка управления оборотами вентиляторов для Huananzhi X99-8M/8MD3/F8/T8/TF](#настройка-управления-оборотами-вентиляторов-для-huananzhi-x99-8m8md3f8t8tf)
14. [Если у вас повреждён сокет](#если-у-вас-повреждён-сокет)

## Инструкция по разблокировке макс.частоты на все ядра, а не на два (unlock)

Перед началом убедитесь, что используете [актуальную версию S3TurboTool](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/S3TurboTool_v1.53cat_S3THv1_DXETHv1_RAWTHv1b.rar)

*(за данную утилиту и новый драйвер анлока особая благодарность ***ser8989***)*

1. Запускаем S3TurboTool и нажимаем MMTool5
2. В появившейся утилите "MMTool Aptio" нажимаем "Load Image" и выбираем биос, в который будем добавлять драйвер анлока
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
10. ***Если система двухпроцессорная, то переходим к созданию DXE драйвера. Для однопроцессорных систем нужно проверить, есть ли в нашем биосе модуль PchS3Peim.*** В S3TurboTool нажимаем UEFITool.
11. В появившейся утилите UEFITool открываем биос
12. Раскрываем список и идём по пути "Intel image > BIOS region > 8C8CE578-...(самый нижний, в котором PEI драйверы) >"
13. Примерно среди первых 20 значений ищем 271DD6F2-... (модуль PchS3Peim). Если такой есть, значит будем собирать PEI драйвер. Если такого нет, значит соберём DXE драйвер.

<details>
<summary>PEI драйвер</summary>

(поддерживает режим сна, поддерживает только однопроцессорные системы, не поддерживает брендовые материнские платы):
1. В S3TurboTool нажимаем "Собрать драйвер"
2. Настраиваем необходимые смещения напряжения ([методика нахождения примерных значений](#Подбор-оптимальных-значений-смещения-напряжений-на-вашем-процессоре-undervolting)). Также выбираем, нужен ли дополнительный сигнал при включении и выводе системы из сна. Нажимаем "Собрать драйвер".
3. В S3TurboTool нажимаем UEFITool
4. В появившейся утилите UEFITool открываем биос
5. Раскрываем список и идём по пути "Intel image > BIOS region > 8C8CE578-...(самый нижний, в котором PEI драйверы) >"
6. Находим 271DD6F2-... (модуль PchS3Peim)
7. Правый клик по нему, нажимаем "Replace as is...", выбираем собранный ранее драйвер (находится в папке S3TurboHack)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen01.png)

8. Выбираем "File > Save image file...", сохраняем. Биос готов к прошивке. Прошить можно также соответствующей кнопкой в S3TurboTool.

*Если при попытке прошивки возникает ошибка 103, то следует перезагрузить систему.*

*Если для модификации биоса был использован свой или чужой дамп, то после прошивки нужно сбросить настройки биоса на стандартные (в меню настроек биоса либо перемычкой).*
</details>

<details>
<summary>DXE драйвер</summary>

(не поддерживает режим сна, поддерживает и одно- и двухпроцессорные системы, на двухпроцессорной системе будет работать сон, поддерживает платы ASRock и GIGABYTE):
1. В S3TurboTool нажимаем "Собрать драйвер"
2. Нажимаем в верхнем правом углу кнопку DXE
3. Настраиваем необходимые смещения напряжения ([методика нахождения примерных значений](#Подбор-оптимальных-значений-смещения-напряжений-на-вашем-процессоре-undervolting)). Также выбираем, нужен ли дополнительный сигнал при включении. Нажимаем "Собрать драйвер".
4. В S3TurboTool нажимаем UEFITool
5. В появившейся утилите UEFITool открываем биос
6. Раскрываем список и идём по пути "Intel image > BIOS region > 8C8CE578-...(предпоследний, второй снизу, в котором DXE драйверы) >"
7. Прокручиваем в самый низ и находим последний DXE драйвер в списке
8. Правый клик по нему, нажимаем "Insert after...", выбираем собранный ранее драйвер (находится в папке DXETurboHack)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen02.png)

9. Выбираем "File > Save image file...", сохраняем. Биос готов к прошивке. Прошить можно также соответствующей кнопкой в S3TurboTool.

*Если при попытке прошивки возникает ошибка 103, то следует перезагрузить систему.*

*Если для модификации биоса был использован свой или чужой дамп, то после прошивки нужно сбросить настройки биоса на стандартные (в меню настроек биоса либо перемычкой).*
</details>

<details>
<summary>RAW драйвер</summary>

(поддерживает режим сна, поддерживает только однопроцессорные системы, не поддерживает анлок режима AVX):

***Китайские материнские платы не нуждаются в этом драйвере и для них не рекомендуется делать анлок данным способом. Только для крайних случаев, для необычных брендовых материнских плат.***

*ВНИМАНИЕ! После установки RAW драйвера не редактируйте этот биос любыми программами, в противном случае биос станет неработоспособным.*
1. В S3TurboTool нажимаем "Собрать драйвер"
2. Нажимаем в верхнем правом углу кнопку RAW и выбираем необходимый биос
3. Настраиваем необходимые смещения напряжения. Нажимаем "Собрать и установить драйвер".
4. Биос сохранился в ту же папку и готов к прошивке. Прошить можно также соответствующей кнопкой в S3TurboTool.

*Если при попытке прошивки возникает ошибка 103, то следует перезагрузить систему.*

*Если для модификации биоса был использован свой или чужой дамп, то после прошивки нужно сбросить настройки биоса на стандартные (в меню настроек биоса либо перемычкой).*
</details>

***При возникновении трудностей, а также если у Вас есть замечания и пожелания, обращайтесь в [Telegram группу](https://t.me/russian_xeon_community)***

## [Видео-инструкция](https://youtu.be/2hfhdrIrXR4) *(спасибо Zerg_fb)*:

[![](https://i.ytimg.com/vi/2hfhdrIrXR4/mqdefault.jpg)](https://youtu.be/2hfhdrIrXR4)

## Instructions for unlocking the maximum frequency for all cores, not two (unlock)

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
10. ***If the system is dual-processor, then proceed to creating the DXE driver. For uniprocessor systems, you need to check if there is a PchS3Peim module in our BIOS.*** In S3TurboTool, click UEFITool.
11. In the UEFITool utility that appears, open the BIOS
12. We open the list and follow the path "Intel image > BIOS region > 8C8CE578-...(the lowest one, in which the PEI drivers) >"
13. We are looking for 271DD6F2-... (PchS3Peim module) among the first 20 values. If there is one, then we will build the PEI driver. If this is not the case, then we will build the DXE driver.

<details>
<summary>PEI driver</summary>

(supports sleep mode, only supports uniprocessor systems):

1. In S3TurboTool, click "Собрать драйвер"
2. We adjust the required voltage offsets ([method for finding approximate values](#Finding-the-optimal-voltage-offset-values-for-your-processor-undervolting)). We also choose whether an additional signal is needed when turning on and waking the system from sleep. Click "Собрать драйвер".
3. In S3TurboTool, click UEFITool
4. In the UEFITool utility that appears, open the BIOS
5. We open the list and follow the path "Intel image > BIOS region > 8C8CE578-...(the lowest one, in which the PEI drivers) >"
6. Find 271DD6F2-... (PchS3Peim module)
7. Right click on it, click "Replace as is...", select the previously assembled driver (located in the S3TurboHack folder)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen01.png)

8. Choose "File > Save image file...", save. BIOS is ready for flashing. You can also flash it with the corresponding button in S3TurboTool.

*If error 103 occurs when trying to flash, then you should reboot the system.*

*If you used your own or someone else's dump to modify the BIOS, then after flashing, you need to reset the BIOS settings to the standard ones (in the BIOS settings menu or using a jumper).*
</details>

<details>
<summary>DXE driver</summary>

(does not support sleep mode, supports single and dual processor systems):
1. In S3TurboTool, click "Собрать драйвер"
2. Press the DXE button in the upper right corner
3. We adjust the required voltage offsets ([method for finding approximate values](#Finding-the-optimal-voltage-offset-values-for-your-processor-undervolting)). We also choose whether an additional signal is needed when turning on. Click "Собрать драйвер".
4. In S3TurboTool, click UEFITool
5. In the UEFITool utility that appears, open the BIOS
6. Expand the list and follow the path "Intel image > BIOS region > 8C8CE578-...(penultimate, second from the bottom, in which DXE drivers) >"
7. Scroll to the bottom and find the last DXE driver in the list
8. Right click on it, click "Insert after...", select the previously assembled driver (located in the DXETurboHack folder)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen02.png)

9. Choose "File > Save image file...", save. BIOS is ready for flashing. You can also flash it with the corresponding button in S3TurboTool.

*If error 103 occurs when trying to flash, then you should reboot the system.*

*If you used your own or someone else's dump to modify the BIOS, then after flashing, you need to reset the BIOS settings to the standard ones (in the BIOS settings menu or using a jumper).*
</details>

<details>
<summary>RAW driver</summary>

(supports sleep mode, only supports uniprocessor systems, does not support AVX mode unlock):

***Chinese motherboards do not need this driver and it is not recommended to unlock them in this way. Only as a last resort, for unusual branded boards.***

*ATTENTION! After installing the RAW driver, do not edit this BIOS with any programs, otherwise the BIOS will become inoperable.*
1. In S3TurboTool, click "Build Driver"
2. Press the RAW button in the upper right corner and select the required BIOS
3. Adjust the required voltage offsets. Click "Собрать и установить драйвер".
4. The BIOS is saved in the same folder and is ready for flashing. You can also flash it with the corresponding button in S3TurboTool.

*If error 103 occurs when trying to flash, then you should reboot the system.*

*If you used your own or someone else's dump to modify the BIOS, then after flashing, you need to reset the BIOS settings to the standard ones (in the BIOS settings menu or using a jumper).*
</details>

***If you have any difficulties, as well as if you have comments and suggestions, please contact the [Telegram group](https://t.me/russian_xeon_community)***

## Подбор оптимальных значений смещения напряжений на вашем процессоре (undervolting)

**Сначала изучите теорию, потом переходите к инструкции!**

Как известно, Китай платы не позволяют адекватно, в привычном виде, менять напряжение процессора для 26хх v3 (46xx v3) систем. При использовании анлока турбобуста используется снижение напряжения, чтобы процессор работал дольше или на большей частоте. Но снижение напряжения офсетом происходит одновременно во всех режимах (их больше трёх). И самая проблемная часть - напряжение в простое. Таким образом, при подборе драйвера анлока с цифрами, к примеру "V3_MOF_705050.fss" происходит снижение напряжения питания процессора на 0.07V во всех режимах работы, снижение кэша процессора на 0.05V во всех режимах работы. Опыт показал, что нестабильность, в подавляющем большинстве случаев, проявляется в состоянии простоя системы (бездействия). Таким образом, можно без использования анлока, в штатном режиме, снижать частоту процессора офсетом и проводить тестирование, без боязни прошивки биоса и подобных сопутствующих проблем. А уже в дальнейшем, когда будут известны рабочие напряжения простоя процессора, снижать их раз и навсегда биосом с помощью программы (S3TurboTool  Sergey Bakaev ser8989). Изменение будет происходить с помощью софта и при зависании системы или появлении BSOD после перезагрузки все вернётся в исходное состояние.

Подойдут несколько программ для изменения напряжения. Intel XTU, QuickCPU и ThrottleStop. В общем виде инструкция такая: снижаем напряжение процессора, тестируем разными программами в нагрузке, если хорошо, то оставляем на длительное использование. Обычно рабочим снижением будет считаться от 40 до 90, в зависимости от модели процессора и конкретного экземпляра. (для L процессоров может быть рабочим и 120).

Вторым шагом снижаем напряжение на кэше процессора (Cache). Обычно в драйверах выбирается значение 50-60, но на практике оно легко может превышать и 120. Нестабильность проверять программой LinX с AVX-инструкциями и программами, сильно нагружающими память (к примеру TestMem5).

После нахождения значений, при которых система стабильно работает как в простое, так и в нагрузке, можно данные значения прошивать в биос материнской платы.

*(автор ***Василий Пупкин***)*

---

У каждой модели процессора есть значение TDP, это максимальная его мощность в ваттах. В некоторых задачах происходит достижение этого предела, и процессор начинает уменьшать частоту на ядрах шагами по 100МГц, чтобы не перейти за эту грань. Мощность, это напряжение, умноженное на ток. Понижая напряжение, достигается улучшение энергоэффективности процессора, т.е. при той же нагрузке снижается потребление энергии и как следствие тепловыделение. Соответственно, при той же граничной нагрузке, процессор сможет более уверенно удерживать частоту.

Затронем два блока в наших процессорах - Core (ядра) и Cache (кэш) (есть ещё блок SystemAgent, но снижение на нём напряжения, не даёт снижения потребления процессора, поэтому лучше не менять его).

Напряжение понижается методом его смещения (offset) на определённую величину. Т.е. во всех режимах работы процессора напряжение будет меньше на заданное нами значение. Можно безопасно произвести поиск значений смещения на ядрах на системе без анлока. Можно сделать это и на системе с активным анлоком, в таком случае при изменении смещения, анлок пропадёт. Это никак не повлияет на поиск значения, т.к. у ядер напряжение меняется в зависимости от его частоты.

***Перед дальнейшими действиями, сохраните важные данные в вашей системе, будьте готовы к возможным зависаниям или синему экрану!
Перед запуском тестов позаботьтесь о должном охлаждении процессора, области VRM и оперативной памяти (она может троттлить без дополнительного охлаждения)!***

1. Подготовка: скачивание необходимых программ - для изменения смещения напряжения будем использовать [ThrottleStop](https://www.techpowerup.com/download/techpowerup-throttlestop/), для тестирования стабильности ядер - программу [OCCT](https://www.ocbase.com/download), для теста стабильности кэша - [LinX v0.6.5](https://github.com/sanekgusev/LinX-old/releases/latest)

**Подбор оптимального значения смещения напряжения на ядра**

1. Запускаем ***ThrottleStop***, нажимаем ***FIVR***
2. В блоке ***FIVR Control*** отмечаем ***CPU Core***
3. В блоке ***CPU Core Voltage*** отмечаем ***Unlock Adjustable Voltage***, понижаем ***Offset Voltage***, начинаем с ***-100mV*** и нажимаем ***Apply***. Если система зависла или мы увидели синий экран, значит такое смещение нам точно не подходит, перезагружаем систему и пробуем -95mV, -90mV и т.д.
4. Если всё вроде бы стабильно, значит запускаем ***OCCT***, выставляем режим теста:
    - CPU+RAM
    - Data Set: Large
    - Mode: Extreme
    - Load Type: Variable
    - Instruction Set: SSE
    - Thread Settings: Auto

5. Запускаем тест. Если система зависла или мы увидели синий экран (обычно ошибка CLOCK_WATCHDOG_TIMEOUT), то уменьшаем смещение и продолжаем тестировать до появления стабильности (***1 час***). Это и будет нашим значением для ядер.

**Подбор оптимального значения смещения напряжения на кэш**

1. Запускаем ***ThrottleStop***, нажимаем ***FIVR***
2. В блоке ***FIVR Control*** отмечаем ***CPU Cache***
3. В блоке ***CPU Cache Voltage*** отмечаем ***Unlock Adjustable Voltage***, понижаем ***Offset Voltage***, начинаем с ***-125mV*** и нажимаем ***Apply***. Если система зависла или мы увидели синий экран, значит такое смещение нам точно не подходит, перезагружаем систему и пробуем -120mV, -115mV и т.д.
4. Если всё вроде бы стабильно, значит запускаем ***LinX***, настраиваем тест:
    - Память: 8192МБ (или меньше, если свободной памяти меньше)
    - Выполнять: 10 минут

5. Запускаем тест. При нестабильности возможен синий экран (обычно ошибка ***WHEA_UNCORRECTABLE_ERROR***), если это происходит, то уменьшаем смещение и продолжаем тестировать до появления стабильности. Это и будет нашим значением для кэша.

Т.к. для проверки кэша нужны плотные потоки данных, то в дальнейшем рекомендуется ещё раз провести тест, но уже с анлоком вкупе с багом SVID/FIVR (уделите особое внимание температуре во время теста)

*(помощь в составлении инструкции ***Data Name ID***)*

## Finding the optimal voltage offset values ​​for your processor (undervolting)
---
As you know, China boards do not allow to adequately, in the usual form, change the processor voltage for 26xx v3 (46xx v3) systems. When using a turbo-boost unlock, a voltage reduction is used to make the processor work longer or at a higher frequency. But voltage reduction by offset occurs simultaneously in all modes (there are more than three of them). And the most problematic part is stress in idle time. Thus, when selecting an unlock driver with numbers, for example "V3_MOF_705050.fss", the processor voltage decreases by 0.07V in all operating modes, the processor cache decreases by 0.05V in all operating modes. Experience has shown that instability, in the overwhelming majority of cases, manifests itself in a system idle state (inactivity). Thus, it is possible, without using an unlock, in the normal mode, to reduce the processor frequency by offset and conduct testing without fear of BIOS firmware and similar related problems. And in the future, when the processor idle operating voltages are known, reduce them once and for all by the BIOS using the program (S3TurboTool Sergey Bakaev ser8989). The change will occur with the help of software, and when the system hangs or reboots, everything will return to its original state.

Several programs are suitable for changing the voltage. Intel XTU, QuickCPU and ThrottleStop. In general, the instruction is as follows: we lower the processor voltage, test it with different programs in the load, if it is good, then we leave it for long-term use. Typically, a working degradation will be considered from 40 to 90, depending on the processor model and specific instance. (for L processors, 120 can be working).

The second step is to lower the voltage on the processor cache. Usually the driver chooses a value of 50-60, but in practice it can easily exceed 120. Check the instability with the LinX program with AVX-instructions and programs that heavily load memory (for example TestMem5).

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
4. If everything seems to be stable, then we launch the [OCCT](https://www.ocbase.com/download), set the test mode:
    - CPU+RAM
    - Data Set: Large
    - Mode: Extreme
    - Load Type: Variable
    - Instruction Set: SSE
    - Thread Settings: Auto

5. We run the test. If the system freezes or we see a blue screen (usually the CLOCK_WATCHDOG_TIMEOUT error), then we reduce our offset and continue testing until stability appears (1 hour). This will be our core value.

Next, we move on to the cache test (you can start with -125mV). Optimally test the cache in LinX (tested on [v0.6.5](https://github.com/sanekgusev/LinX-old/releases/latest)), set the 8192MB memory setting, press Start. In case of instability, a blue screen is possible (usually a WHEA_UNCORRECTABLE_ERROR error). 5 minutes of the test is enough.

Because to check the cache, dense data streams are needed, then it is recommended to run the test again in the future, but this time with unlock, coupled with the SVID/FIVR bug (pay special attention to the temperature during the test)

## Ultimate Patcher Tool

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
- Huananzhi X99-8M/8MD3/F8/T8/TF (25.05.2020 - всё работает) (17.08.2020 и новее - !НЕ РАБОТАЕТ! - пост-код 79)
- Huananzhi X99-F8D/T8D (24.06.2020 - всё работает, кроме смартфана)
- Jginyue X99M-PLUS D3 (03.11.2020 - всё работает, кроме смартфана)

Список будет пополняться, сообщайте о результатах в [Telegram группу](https://t.me/russian_xeon_community)

## Восстановление биоса программатором
Процессор, оперативную память и элемент питания CR2032 извлекать необязательно. В некоторых случаях необходим подключенный БП, для дежурного напряжения (система должна быть выключена при этом).

Huananzhi X99-F8/T8/TF требуется извлечь из корпуса, и снять радиатор с хаба, т.к. крепится он винтами сзади.

Далее будет приведена инструкция на примере распространённого программатора CH341A.

1. Прищепку вставляем в программатор, в секцию для 25 серии SPI микросхем, соблюдая ключ. Красный провод должен быть в том же углу, где точка на изображении микросхемы.
2. Надеваем прищепку на саму микросхему, также соблюдая совмещение красного провода с точкой на ней.
3. Подключаем программатор к ПК.
4. Скачиваем [NeoProgrammer](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/blob/master/NeoProgrammer_2.2.0.10.zip) и распаковываем архив.
5. Заходим в Диспетчер устройств. Устанавливаем необходимые драйвера (выбираем папку с NeoProgrammer).
6. Запускаем NeoProgrammer, жмём иконку со знаком вопроса (Определить чип). Должно произойти определение модели микросхемы. Если определения не происходит, значит имеем плохой контакт с микросхемой (обычно между прищепкой и микросхемой). В случае успеха выбираем модель из предложенного списка. Иногда точно такой же нет, достаточно выбрать что-то похожее. Теперь мы можем прочитать содержимое микросхемы биоса, если оно нужно, нажав иконку "Читать чип". Для сохранения в файл нажимаем иконку "Сохранить файл".
8. Для прошивки, сначала стираем содержимое, нажав на иконку "Стереть". Далее открываем в программе файл рабочего биоса и жмём иконку "Записать". Процесс прошивки может длиться от нескольких минут до часа.
9. После прошивки рекомендую прочитать содержимое микросхемы биоса, сохранить в файл, и сравнить с оригиналом по контрольной сумме.

## Настройка таймингов оперативной памяти

[Видео-инструкция](https://youtu.be/2UdU4n6B0V8). Рекомендуется к освоению.

[![](https://i.ytimg.com/vi/2UdU4n6B0V8/mqdefault.jpg)](https://youtu.be/2UdU4n6B0V8)

*(спасибо Ilyme)*

Также есть ещё одна инструкция.

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

**Если старт прошел успешно, то проверяем объём памяти. Если он сократился, то данная конфигурация нестабильна. Если всё в порядке, то запускаем [testmem5 с профилем Extreme1 @anta777](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/blob/master/TM5.zip) и ждём не менее 20 минут.**

Задать любой возникший вопрос по разгону памяти можно в [Telegram группе](https://t.me/russian_xeon_community)

## Что делать, если пропадает загрузка системы

Столкнуться с данной проблемой можно, если система установлена на диск со стилем разделов GPT. Обычно после сброса биоса первая загрузка выполняется нормально, а после перезагрузки видим ошибку, т.к. загрузчик вовсе пропадает.

Для решения данной проблемы переходим в меню "Advanced > CSM Configuration" и меняем значение пункта "Boot option filter" на "UEFI only"

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen11.png)

## Как открыть скрытые пункты меню настроек биоса

Для этого понадобится программа AMIBCP (имеется в составе [S3TurboTool](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/S3TurboTool_v1.53cat_S3THv1_DXETHv1_RAWTHv1b.rar))

Открываем в ней файл биоса и видим структуру меню. Находим необходимый скрытый пункт меню и меняем значение параметра "Access/Use" с "Default" на "USER". Если данный раздел меню также является скрытым, то переходим в вышестоящий раздел и открываем к нему доступ.

Также есть [видео-инструкция](https://youtu.be/BddkZIsLN_k)

[![](https://i.ytimg.com/vi/BddkZIsLN_k/mqdefault.jpg)](https://youtu.be/BddkZIsLN_k)

## Добавление поддержки Resizable BAR в биос

1. Скачиваем [ReBarDxe.ffs и ReBarState.exe](https://github.com/xCuri0/ReBarUEFI/releases/latest)
2. Открываем в [UEFITool 0.28.0](https://github.com/LongSoft/UEFITool/releases/tag/0.28.0) файл биоса
3. Раскрываем список и идём по пути "Intel image > BIOS region > 8C8CE578-...(предпоследний, второй снизу, в котором DXE драйверы) >"
4. Прокручиваем в самый низ и находим последний DXE драйвер в списке
5. Правый клик по нему, нажимаем "Insert after...", выбираем скачанный ранее файл ReBarDxe.ffs
6. Выбираем "File > Save image file...", сохраняем. Прошиваем.
7. Активируем настройку Above4GDecoding
8. Из под Windows запускаем ReBarState.exe и вводим две цифры "32" и нажимаем ввод. Перезагружаемся.
9. В [GPU-Z](https://www.techpowerup.com/download/techpowerup-gpu-z/) можно проверить состояние ReBar, в т.ч. выполнение каждого условия для активации

Также есть [видео-инструкция](https://youtu.be/CR7QV135ANw)

[![](https://i.ytimg.com/vi/CR7QV135ANw/mqdefault.jpg)](https://youtu.be/CR7QV135ANw)

## Отключение бипера

***Старый метод, крайне редко работающий, поэтому рекомендую сразу обращаться в [Telegram группу](https://t.me/russian_xeon_community).***

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

*Если бипер по-прежнему издаёт звуки, то попробуйте также обработать модуль StatusCodePei (в разделе с PEI модулями).*

## Поддержка

Если Вы имеете желание отблагодарить кого-либо, например если кто-то вам помог, то вот реквизиты:

ser8989 - 5366-7280-9045-9598 (МТС Банк)

Василий Пупкин - 5368-2902-1226-6714 (ВТБ)

S.T.A.L.K.E.R - 5469-6700-1549-2427 (Сбербанк)

Снуки Лоу - 4817-7602-3407-9170 (Сбербанк)

Костян - 4276-0200-1999-5122 (Сбербанк)

Всем большое спасибо за помощь!

---

## О пост-кодах

Помните, что при любой проблеме стоит осмотреть сокет на наличие повреждений и очистить контакты процессора, если это лично вы не делали и не уверены в этом. Также пробуйте сбросить настройки биоса перемычкой.

Известные решения при определённых пост-кодах:

* FF - что угодно; если система только что собрана, то стоит осмотреть сокет на наличие повреждений и очистить контакты процессора; если система до этого работала, и сброс настроек биоса перемычкой не помогает, то стоит прошить биос; если система после запуска сразу выключается, успев показать только FF, то это значит что система не может запустить БП, либо БП сразу уходит в защиту по какой-то причине
* 19 - встречается при попытке разгона разгонных моделей процессоров на неподходящем для этого биосе; в иных случаях стоит прошить биос
* CD - установлен V4 ES (они несовместимы)
* DC - установлено два разных процессора в двухсокетной системе
* D6 - проблема с видеокартой
* B2 - проблема с инициализацией видеокарты
* AE - ожидание загрузочного диска (если при этом чёрный экран, значит проблема в режиме видеовывода)
* 53 - оперативная память не установлена
* B7, BF - проблема с оперативной памятью (пробуйте сброс настроек биоса перемычкой)
* B0 - оперативная память установлена не в тот слот (нарушена последовательность заполнения слотов)
* 92 - система запускается в видеорежиме UEFI, но такой режим видеокартой не поддерживается (попробуйте сбросить настройки биоса перемычкой)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen06.png)
![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen07.png)
![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen08.png)

## Если вы наблюдаете частоту системной шины меньшую, чем 99,75МГц
Например, 98МГц. В таком случае вы не получите 100МГц, даже если прошьёте соответствующий биос. Насколько известно на данное время, частоту занижает активный Hyper-V в ОС.

*(спасибо ***Василию Пупкину***)*

Также помогает отключение компонента "Песочница Windows"

*(спасибо ***C0oo1D***)*

## Настройка управления оборотами вентиляторов для Huananzhi X99-8M/8MD3/F8/T8/TF

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

## Если у вас повреждён сокет

Аккуратно править ножки сокета, например, зубочисткой или подобным предметом.

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen09.png)

![](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods/raw/master/.git_images/screen10.png)

На изображениях выше отмечены ножки, отсутствие двух-трёх которых некритично, т.к. ножки многократно продублированы. Если у вас необратимо повреждены другие ножки, тоже может быть шанс на успешную работу системы, для этого необходимо свериться с распиновкой или написать в [Telegram группу](https://t.me/russian_xeon_community).