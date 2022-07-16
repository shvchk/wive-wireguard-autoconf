### Подготовка

1. **Термины**

    - **Текстовый терминал (текстовая консоль)**: программа для работы с текстовым интерфейсом, через которую текстовые программы принимают и выводят информацию. Далее просто "терминал".

    - **Оболочка командной строки (шелл)**: программа, которая выполняет текстовые команды.

    - **SSH**: безопасный способ подключения к удалённому устройству и выполнения команд на нём.

    - **WireGuard**: один из способов (протоколов) организации VPN. Относительно новый, но популярный из-за простоты и производительности.


2. **Запуск терминала и работа в шелле**

    Этот подраздел можно пропустить (кроме замечания по поводу ошибки в Windows 10 внизу) и вернуться только в случае возникновения затруднений.

    В Windows предустановлен терминал и шелл Windows PowerShell. В Windows 11 также предустановлен более современный терминал: Терминал Windows / Windows Terminal (его можно [установить и в Windows 10](ms-windows-store://pdp/?ProductId=9N0DX20HK701)). Запустить терминал можно, нажав правой кнопкой мыши по кнопке Пуск > Терминал Windows или Windows Terminal (в Windows 10 — Windows PowerShell) или найдя его в меню Пуск по одному указанных выше названий. Терминал также можно запустить в определённой папке в Проводнике, кликнув правой кнопкой мыши по свободному месту в нужной папке (в Windows 10 — с зажатым `Shift`) > Открыть в терминале (в Windows 10 — Открыть окно PowerShell здесь).

    Роутер работает на Linux, там другой шелл, но описанные ниже принципы работы универсальны.

    В терминал можно вставить текст из буфера обмена (например, команду) по нажатию правой кнопки мыши. Выделенный в терминале текст можно скопировать в буфер обмена также по нажатию правой кнопки мыши.

    Курсор ввода перемещается стрелками `←` `→`. Зажатие `Ctrl` позволяет переместить курсор не на 1 символ, а на 1 слово. `Home` перемещает курсор в начало строки, `End` — в конец. `Backspace` удаляет символ перед курсором, `Delete` — после.

    Стрелками `↑` `↓` можно перемещаться по истории команд: `↑` покажет предыдущую команду, `↓` — более позднюю (при наличии).

    Текущая папка отображается слева от строки для ввода команд. Например, `PS C:\Users\X> ` означает, что мы находимся в папке `C:\Users\X`.

    При вводе команд и названий файлов / папок `Tab` производит автодополнение. Например, для файла `hello-world.txt` в текущей папке достаточно ввести первые несколько букв (скажем, `hell`) и нажать `Tab` — терминал автоматически дополнит имя. В случае наличия нескольких команд / названий с введёнными первыми буквами, последовательные нажатия `Tab` будут выводить их по очереди, так что если не подошёл вариант автодополнения, просто нажми `Tab` снова.

    Перейти в другую папку можно командой `cd путь/к/папке`. Например, `cd c:/users/x`. В Linux разделителем папок в пути является `/` (слэш), в Windows — по умолчанию `\` (обрантый слэш, бэкслэш), но можно использовать и `/` (слэш). Регистр символов в названиях команд, папок и файлов важен в Linux (т.е. файл `test.txt` и `Test.txt` — разные файлы), в Windows не важен. Для папок / файлов, находящихся в текущей папке, путь к ней можно опустить и писать только путь относительно неё. Текущая папка в относительном пути также может быть обозначена `.` (обычно это не требуется, но может встретиться при автодополнении). Пути с пробелами лучше заключать в кавычки.

    Выполнение длительной команды можно остановить, нажав `Ctrl + C`. Команда `exit` завершит работу шелла (и закроет окно терминала, если шелл был в нём единственным).

    ---

    ⚠️ Windows PowerShell в Windows 10 содержит ошибку, иногда не позволяющую вводить заглавные буквы. Для её исправления нужно обновить модуль PSReadLine.

    <details>
      <summary>Обновление PSReadLine</summary>

      - Запусти терминал от имени администратора

      - Обнови модуль PSReadLine:

        ```ps
        install-module psreadline -force
        ```

      - В случае запроса на установку поставщика NuGet согласись, ответив `Y`

      - После завершения установки выполни команду `exit` для выхода из шелла и терминала

      - При следующем запуске терминала шелл запросит подтверждение на запуск нового модуля. Согласись, ответив `A`.
    </details>

3. **Установка клиента SSH**

    В современных версиях Windows SSH клиент предустановлен. Чтобы удостовериться в этом, нажми Пуск > Параметры > Приложения > Дополнительные компоненты > введи SSH в поле поиска

    - Если найден компонент Клиент OpenSSH, значит, он уже установлен и дополнительных действий не требуется

    - Если компонент Клиент OpenSSH не найден, нажми вверху Добавить компонент, найди Клиент OpenSSH, отметь его галочкой и нажми Установить


### Настройка WireGuard соединения на роутере

Клиент WireGuard на роутере будет подключаться к серверу WireGuard в интернете. Для подключения к серверу WireGuard обычно используется конфигурационный файл. Его устройство для этой инструкции не важно, необходимо лишь его наличие, но если интересно, см. скрытый раздел ниже.

<details>
  <summary>Подробнее о конфигурационном файле WireGurad</summary>

  Используется конфигурационный файл в формате INI:

  ```ini
  [Раздел]
  Параметр = значение
  ```

  Содержание его примерно следующее (ниже разберём):

  ```ini
  [Interface]
  Address = 10.10.10.11/32
  PrivateKey = QMZwEV/dfa5ZpTfcVv3T6mSu5/Jw3yUEhlB9er75928=

  [Peer]
  PublicKey = 2t8EW0DTKafCpnQTAi7xoOH4qOUljp9COLTtKIR+wD4=
  Endpoint = 12.34.56.78:51820
  AllowedIPs = 0.0.0.0/0
  ```

  В разделе `[Interface]` находятся сведения о нашем устройстве (т.е. в нашем случае о клиенте):
  - `Address` — адрес в VPN-сети. Может быть указано несколько через запятую.
  - `PrivateKey` — ключ, заканчивается символом `=`

  В разделе `[Peer]` находятся сведения об удалённом устройстве, с которым мы устанавливаем VPN-соединение (т.е. в нашем случае о сервере):
  - `PublicKey` — ключ, заканчивается символом `=`
  - `Endpoint` — адрес и порт для подключения в формате `адрес:порт`
  - `AllowedIPs` — адреса, с которыми можно обмениваться данными через это соединение. Может быть указано несколько через запятую.

  Заметь, значения не содержат пробелов.

  Значения этих параметров могут понадобиться для ручной настройки WireGuard соединения на роутере. В конфигурационном файле могут быть и другие параметры, но они как правило опциональны и далее не потребуются.
</details>

Для подключения к роутеру будем использовать SSH. Для доступа по SSH используются те же данные (адрес, логин и пароль), что и для входа в веб-интерфейс.

1. В файловом менеджере перейди в папку, где лежит файл конфигурации WireGuard  
    Предположим, этот файл называется `wireguard.conf` (скорректируй на своё).  
    Если у файла не отображается расширение (точка и несколько символов в конце названия файла), включите его отображение: [Windows 10](https://remontka.pro/file-extensions), [Windows 11](https://remontka.pro/show-file-extensions-windows-11)

2. Открой терминал в этой папке  
    Cм. Запуск терминала и работа в шелле: клик правой кнопкой мыши по свободному месту в папке (в Windows 10 — с зажатым `Shift`) > Открыть в терминале (в Windows 10 — Открыть окно PowerShell здесь)

3. Скопируй файл конфигурации на роутер по SSH:

    ```sh
    scp wireguard.conf Admin@192.168.1.1:/tmp/wg-client-autoconf
    ```

    - При первом подключении по SSH необходимо подтвердить доверие удалённому устройству, ответив `yes` на запрос Are you sure you want to continue connecting [...] ?

    - Далее последует запрос пароля, введи пароль. Вводимые символы пароля не будут отображаться вообще, это намеренная предосторожность.

4. Подключись к роутеру по SSH для выполнения команд на нём:

    ```sh
    ssh Admin@192.168.1.1
    ```

5. В случае успешного подключения запустится шелл роутера. Его строка для ввода команд выглядит примерно так:

    ```
    [Wive-NG-HQ:~]#
    ```

    Далее команды выполняются в шелле роутера.

6. Запусти программу автонастройки клиента WireGuard:

    ```sh
    wget -qO- https://github.com/shvchk/wive-wireguard-autoconf/raw/main/conf.sh | sh    
    ```

7. Программа проверит, не устарело ли ПО роутера, и при необходимости потребует его обновить. После обновления ПО роутер автоматически перезагрузится и SSH соединение с ним будет разорвано, тогда необходимо будет вернуться к п. 1 этого раздела инструкции.

8. Программа настроит соединение, используя загруженный файл конфигурации, и предложит его проверить.

    ```
    Настройка завершена, проверить соединение WireGuard? [ введите + или - ]:
    ```

    Согласись, ответив `+`

9. Программа спросит, оставить ли соединение включенным, настроить ли автозапуск и выборочную маршрутизацию для обхода блокировок. Согласись с каждым из этих предложений, ответив `+`

10. Готово.  
    Для выхода из шелла роутера выполни команду `exit` или просто закрой терминал.
