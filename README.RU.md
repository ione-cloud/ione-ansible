ione-ansible

# tr_ru

## Администраторская часть

### Playbooks

#### Создать | Create
Создать плейбук(после применения формы записывается в БД). Поля:
* Имя - строка длинной до 128 символов
* Описание - текст, можно написать что угодно. Удобно будет описывать каждое отдельное "задание"(task) плейбука
* Тело - тело плейбука в формате YAML. Дополнительные требования к синтаксису:
    * Объект должен быть массивом плейбуков, т.е. начинаться с " - hosts:"
    * hosts должен содержать только <%group%>, чтобы дальнейшие запуски плейбуки прошли корректно
    * Если плейбук содержит поле vars(переменные), каждая переменная внутри этого поля должна иметь стандартное значение
> Проверить синтаксис можно и нужно по нажатию кнопки "Проверить синтаксис"(Check syntax)

При записи плейбука, автоматически будут заполнены следующие поля:
* ID пользователя и группы пользователья создающего плейбук в качестве владельца(Owner) и группы(Group) 
* Права 700(**rwx\-\-\-\-\-\-**)
* Время записи

#### Изменить | Update
Данным действием можно изменить данные из полей плейбука
> Требуется уровень доступа **MANAGE**

#### Запустить | Run
Откроет [AnsiblePlaybookProcess.create](#label-Создать+|+Create)
> Требуется уровень доступа **USE**

#### Изменить имя           | Rename
Можно изменить имя плейбука используя кнопку *edit* возле имени плейбука и введя новое имя в появившейся форме.
> Требуется уровень доступа **MANAGE**

#### Изменить права доступа | CHMOD
Можно изменить права доступа к плейбуку используя таблицу чекбоксов видимую при просмотре отдельного плейбука.
Права делятся на 3 уровня доступа:
* Использовать      | Use
* Управлять         | Manage
* Администрировать  | Admin

И на 3 группы:
* Владелец      | Owner     | Владелец плейбука
* Группа        | Group     | Пользователи в той же группе что и владелец плейбука
* Остальные     | Others    | Все остальные пользователи

#### Изменить владельца     | CHOWN
Можно изменить владельца плейбука нажав кнопку *edit* возле имени владельца плейбука и выбрав нового владельца из выпадающего списка.
> Требуется уровень доступа **ADMIN**

#### Изменить группу        | CHGRP
Можно изменить группу плейбука нажав кнопку *edit* возле имени группы плейбука и выбрав новую группу из выпадающего списка.
> Требуется уровень доступа **ADMIN**

#### Удалить                | Delete
Данное действие удалит плейбук из БД
> Требуется уровень доступа **ADMIN**


### Processes

#### Создать    | Create
Создает сущность процесс и записывает его в БД.
В этой сущности хранится:
* proc_id - уникальный идентификатор сущности в БД
* install_id - уникальный идентификатор для использования при запуске
* playbook_id - ID плейбука, который был запущен
* uid - ID пользователя создавшего процесс
* create_time - время создания процесса
* start_time - время запуска процесса
* end_time - время завершения процесса
* status - статус процесса(один из восьми: PENDING - создан, RUNNING - запущен и работает, SUCCESS - завершен успешно, CHANGED - большинство кодов завершения равны changed, UNREACHABLE - большинство хостов недоступны, FAILED - есть коды завершения равные failed, LOST - невозможно найти лог, DONE - удален)
* log - текстовый лог работы плейбука
* hosts - список ID машин и их IP адресов с портами, где должен быть запущен или был запущен плейбук
* vars - переменные переданные в плейбук
* playbook_name - имя плейбука
* runnable - тело плейбука с данными значениями переменных
* comment - комментарий к процессу
* codes - коды завершения (ok, unreachable, changed, failed)


#### Запустить  | Run
Запускает процесс выполнения на хостах hosts

#### Удалить    | Delete
Устанавливает статус плейбука на **DONE**