### Huananzhi X99-T8
### BIOS 2021/09/15 (D3X99302)

    Тайминг Command Rate (CR) имеет значение 3 и никак не изменяется в настройках (на данный момент неизвестно как исправить / рекомендуется биос от iEngineer от модели X99-F8)
	
Известные изменения в сравнении с версией 2021/05/20:

    + Новое лого бренда при запуске системы

*v002:*
* - убран фикс шины PCI-E, т.к. некоторые видеокарты вообще перестали стартовать
* обновлены микрокоды
* настроено управление оборотами вентилятора процессора (<45°=30%/>80°=100%)
* открыты пункты меню "CPU C State Control", "SOCKET RAPL Config", "Per-Socket Configuration", "OverClocking Feature", "Program PP0_CURT_CFG_CTRL_MSR" и "PCI Subsystem Settings"
* открыт доступ к настройке таймингов (название пункта "Memory Timings & Voltage Override")
* BCLK 100.00MHz (0.00% SSC)

*v001:*
* обновлены микрокоды
* исправлена проблема с шиной PCI-E, когда она работала в режиме 1.1 на некоторых видеокартах *спасибо Снуки Лоу*
* настроено управление оборотами вентилятора процессора (<45°=30%/>80°=100%)
* открыты пункты меню "CPU C State Control", "SOCKET RAPL Config", "Per-Socket Configuration", "OverClocking Feature", "Program PP0_CURT_CFG_CTRL_MSR" и "PCI Subsystem Settings"
* открыт доступ к настройке таймингов (название пункта "Memory Timings & Voltage Override")
* BCLK 100.00MHz (0.00% SSC)
