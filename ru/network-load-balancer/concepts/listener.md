# Обработчик трафика

*Обработчик трафика* — это распределенный по всем [зонам доступности](../../overview/concepts/geo-scope.md) компонент, который принимает входящий трафик на балансировщик и распределяет его по всем работающим ресурсам в целевых группах, подключенных к балансировщику. 

Для одного балансировщика можно создать несколько обработчиков трафика. 

Все обработчики трафика на одном балансировщике всегда используют только один IP-адрес. Использовать несколько IP-адресов на одном балансировщике нельзя.

Обязательными атрибутами каждого обработчика трафика являются:
* `Имя` — используется для именования обработчика и связи его с другими компонентами балансировщика.
* `IP-адрес` — IPv4-адрес, на который балансировщик будет принимать входящий трафик. Протокол IPv6 в настоящее время не поддерживается. IP-адрес обработчика будет анонсироваться как `/32`-префикс из всех [зон доступности](../../overview/concepts/geo-scope.md) {{ yandex-cloud }}.
Таким образом, при выходе из строя одной зоны доступности, входящий трафик на IP-адрес обработчика будет перенаправлен сетевым оборудованием в другие работающие зоны доступности.

    {% note info %}

    IP-адрес для обработчика трафика может быть назначен:
    * Внешнему балансировщику — автоматически из пула публичных адресов или из списка [зарезервированных IP-адресов](../../vpc/operations/get-static-ip.md).
    * Внутреннему балансировщику — автоматически из пула адресов. Зарезервировать внутренний IP-адрес из адресного пространства {{ vpc-short-name }} для обработчика трафика нельзя. 

    {% endnote %}

* `Протокол` - тип транспортного протокола, трафик которого будет принимать обработчик. В настоящее время поддерживаются протоколы TCP и UDP ([поддержка протокола UDP ограничена](#nlb-udp)). Для одного обработчика можно выбрать только один транспортный протокол.
* `Порт` — порт для выбранного транспортного протокола, на котором обработчик будет принимать входящий трафик. Для приема трафика могут использоваться порты из диапазона от 1 до 32767.
* `Целевой порт` — порт, на который ресурсы в целевой группе узлов будут принимать трафик. Может совпадать со значением атрибута `Порт`. После создания обработчика, на виртуальных машинах целевой группы данный порт будет заблокирован для подключений со стороны ресурсов внутри {{ vpc-short-name }}. Виртуальные машины в целевой группе смогут получать трафик только от обработчика трафика сетевого балансировщика.

{% note warning %}

Если вам необходимо использовать на одном балансировщике несколько обработчиков, помните, что порты, на которых обработчики будут принимать трафик, не должны пересекаться между собой. На одном балансировщике два обработчика не могут принимать трафик на одном и том же порту. 

{% endnote %}

Для более гранулярной обработки трафика рекомендуется создавать отдельный сетевой балансировщик для каждого сервиса вместо создания нескольких обработчиков трафика на одном балансировщике.
