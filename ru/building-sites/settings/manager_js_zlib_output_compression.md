---
title: "manager_js_cache_max_age"
translation: "building-sites/settings/manager_js_cache_max_age"
description: "Следует ли включать zlib сжатие для вывода сжатых CSS/JS файлов в Менеджере"
---

-   **Имя**: Включить zlib сжатие вывода файлов JS/CSS для Менеджера   
-   **Тип**: Да/Нет  
-   **По умолчанию**: Нет  
-   **Доступен в**: Revolution 2.2.0+

Следует ли включать zlib сжатие для вывода сжатых CSS/JS файлов в Менеджере. Не включайте это, если вы не уверены, что конфигурационная переменная PHP `zlib.output_compression` может быть установлена в 1. MODX рекомендует оставить ее выключенной. 