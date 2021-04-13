### Huananzhi X99-8M
### BIOS 2020/05/25 (GHX99015)

    + Для серверной памяти активна ECC
    + Присутствует опция настройки таймингов ОЗУ

[Для удаления сигналов бипера](https://github.com/Koshak1013/HuananzhiX99_BIOS_mods#%D0%9E%D1%82%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B1%D0%B8%D0%BF%D0%B5%D1%80%D0%B0), достаточно отключить код только в модуле Bds

*v007:*
* + микрокод для V4 и Realtek UNDI Driver обновлены до актуальных версий
* + открыты пункты меню "SOCKET RAPL Config", "Per-Socket Configuration" и "PCI Subsystem Settings" *спасибо Pavlon и MacArrow*
* добавлено +3.3V в HardwareMonitor
* основа биоса GHX99015
* включена поддержка ASPM
* пункт меню "OverClocking Feature" больше не будет пропадать
* обновлены микрокоды и Realtek Boot Agent GE, добавлен Realtek UNDI Driver, удалено лишнее (Matrox GOP и Intel Gbit)
* настроено управление оборотами вентилятора процессора (<45°=30%/>80°=100%)
* исправлена дата биоса в его настройках
* открыт пункт меню "Program PP0_CURT_CFG_CTRL_MSR"
* пункт меню "Memory Frequency" больше не будет пропадать
* открыт пункт меню "CPU C State Control"
* BCLK 100.00MHz (0.00% SSC)

*v006:*
* + добавлено +3.3V в HardwareMonitor
* + основа биоса GHX99015 *спасибо v111*
* + включена поддержка ASPM
* + пункт меню "OverClocking Feature" больше не будет пропадать *спасибо Vitaliy Pukhov*
* обновлены микрокоды и Realtek Boot Agent GE, добавлен Realtek UNDI Driver, удалено лишнее (Matrox GOP и Intel Gbit)
* настроено управление оборотами вентилятора процессора (<45°=30%/>80°=100%)
* исправлена дата биоса в его настройках
* открыт пункт меню "Program PP0_CURT_CFG_CTRL_MSR"
* пункт меню "Memory Frequency" больше не будет пропадать
* открыт пункт меню "CPU C State Control"
* BCLK 100.00MHz (0.00% SSC)

*v005:*
* + обновлены микрокоды и Realtek Boot Agent GE, добавлен Realtek UNDI Driver, удалено лишнее (Matrox GOP и Intel Gbit) *спасибо v111*
* настроено управление оборотами вентилятора процессора (<45°=30%/>80°=100%)
* исправлена дата биоса в его настройках
* открыт пункт меню "Program PP0_CURT_CFG_CTRL_MSR"
* пункт меню "Memory Frequency" больше не будет пропадать
* открыт пункт меню "CPU C State Control"
* BCLK 100.00MHz (0.00% SSC)

*v004:*
* + настроено управление оборотами вентилятора процессора (<45°=30%/>80°=100%) *спасибо mozg13 и v111*
* + исправлена дата биоса в его настройках
* открыт пункт меню "Program PP0_CURT_CFG_CTRL_MSR"
* пункт меню "Memory Frequency" больше не будет пропадать
* открыт пункт меню "CPU C State Control"
* BCLK 100.00MHz (0.00% SSC)

*v003:*
* + открыт пункт меню "Program PP0_CURT_CFG_CTRL_MSR"
* + пункт меню "Memory Frequency" больше не будет пропадать
* открыт пункт меню "CPU C State Control"
* BCLK 100.00MHz (0.00% SSC)

*v002:*
* + открыт пункт меню "CPU C State Control"
* BCLK 100.00MHz (0.00% SSC)

*v001:*
* + BCLK 100.00MHz (0.00% SSC) *спасибо iEngineer*
