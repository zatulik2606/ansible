# Домашнее задание к занятию 2 «Работа с Playbook»

## Подготовка к выполнению

1. * Необязательно. Изучите, что такое [ClickHouse](https://www.youtube.com/watch?v=fjTNS2zkeBs) и [Vector](https://www.youtube.com/watch?v=CgEhyffisLY).
2. Создайте свой публичный репозиторий на GitHub с произвольным именем или используйте старый.
3. Скачайте [Playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.
4. Подготовьте хосты в соответствии с группами из предподготовленного playbook.

## Основная часть

1. Подготовьте свой inventory-файл `.[prod.yml](https://github.com/zatulik2606/ansible/blob/main/playbook/inventory/prod.yml)
2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает [vector](https://vector.dev). Конфигурация vector должна деплоиться через template файл jinja2. От вас не требуется использовать все возможности шаблонизатора, просто вставьте стандартный конфиг в template файл. Информация по шаблонам по [ссылке](https://www.dmosk.ru/instruktions.php?object=ansible-nginx-install).
3. При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.
4. Tasks должны: скачать дистрибутив нужной версии, выполнить распаковку в выбранную директорию, установить vector.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
~~~
Ошибок нет

root@debian:~/ansible/08-ansible-02-playbook/playbook# ansible-lint site.yml

Passed with production profile: 0 failure(s), 0 warning(s) on 1 files.

~~~
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
~~~
root@debian:~/ansible/08-ansible-02-playbook/playbook# ansible-playbook -i inventory/prod.yml site.yml --check

PLAY [Install Clickhouse] ************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ********************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
ok: [clickhouse-01] => (item=clickhouse-common-static)

TASK [Install clickhouse packages] ***************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01]

TASK [Flush handlers] ****************************************************************************************************************************************

TASK [Delay 20 sec] ******************************************************************************************************************************************
Pausing for 20 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
Press 'C' to continue the play or 'A' to abort
ok: [clickhouse-01]

TASK [Create database] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
skipping: [clickhouse-01]

PLAY [Install Vector] ****************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Get Vector distrib] ************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Install Vector packages] *******************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Deploy config Vector] **********************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

PLAY RECAP ***************************************************************************************************************************************************
clickhouse-01          	: ok=4	changed=0	unreachable=0	failed=0	skipped=1	rescued=0	ignored=0   
vector-01              	: ok=4	changed=0	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0
~~~

7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
~~~
root@debian:~/ansible/08-ansible-02-playbook/playbook# ansible-playbook -i inventory/prod.yml site.yml --diff

PLAY [Install Clickhouse] ************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ********************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
ok: [clickhouse-01] => (item=clickhouse-common-static)

TASK [Install clickhouse packages] ***************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01]

TASK [Flush handlers] ****************************************************************************************************************************************

TASK [Delay 20 sec] ******************************************************************************************************************************************
Pausing for 20 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
Press 'C' to continue the play or 'A' to abort 
ok: [clickhouse-01]

TASK [Create database] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01]

PLAY [Install Vector] ****************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Get Vector distrib] ************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Install Vector packages] *******************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Deploy config Vector] **********************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

PLAY RECAP ***************************************************************************************************************************************************
clickhouse-01              : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-01                  : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0




Проверяем изменения.
[root@f7c022ce6908 ~]# vector --version
vector 0.21.1 (x86_64-unknown-linux-gnu 18787c0 2022-04-22)
[root@f7c022ce6908 ~]# clickhouse-client
ClickHouse client version 22.3.3.44 (official build).
Connecting to localhost:9000 as user default.
Connected to ClickHouse server version 22.3.3 revision 54455.

~~~

8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
~~~
root@debian:~/ansible/08-ansible-02-playbook/playbook# ansible-playbook -i inventory/prod.yml site.yml --diff

PLAY [Install Clickhouse] ************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ********************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
ok: [clickhouse-01] => (item=clickhouse-common-static)

TASK [Install clickhouse packages] ***************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01]

TASK [Flush handlers] ****************************************************************************************************************************************

TASK [Delay 20 sec] ******************************************************************************************************************************************
Pausing for 20 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
Press 'C' to continue the play or 'A' to abort
ok: [clickhouse-01]

TASK [Create database] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [clickhouse-01]

PLAY [Install Vector] ****************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Get Vector distrib] ************************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Install Vector packages] *******************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

TASK [Deploy config Vector] **********************************************************************************************************************************
[WARNING]: sftp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
[WARNING]: scp transfer mechanism failed on [172.17.0.2]. Use ANSIBLE_DEBUG=1 to see detailed information
ok: [vector-01]

PLAY RECAP ***************************************************************************************************************************************************
clickhouse-01          	: ok=5	changed=0	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0   
vector-01              	: ok=4	changed=0	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0 

~~~

9. Подготовьте README.md-файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги. Пример качественной документации ansible playbook по [ссылке](https://github.com/opensearch-project/ansible-playbook).
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-02-playbook` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
