# Подключение к internal host

- Для подключения к `someinternalhost` испольуем SSH *ProxyJump*, для того чтобы не класть ключ на `bastion` используем *AgentForwarding*:
```shell
ssh -i ~/.ssh/appuser -A -J appuser@<bastion_publicIP> appuser@someinternalhost
```

- Для подключения по алиасу нужно сохранить `~/.ssh/config`
```
Host bastion
  HostName <bastion_publicIP>
  User appuser
  IdentityFile ~/.ssh/appuser
  IdentitiesOnly yes
  ForwardAgent yes
  
Host someinternalhost
  HostName someinternalhost
  User appuser
  IdentityFile ~/.ssh/appuser
  IdentitiesOnly yes
  ProxyJump bastion
  
```

Теперь доступно подключение:
```
ssh bastion
ssh someinternalhost
```

# Валидный сертификат pritunl

Воспользуемся сервисом [sslip.io](https://sslip.io)  (на [nip.io](https://nip.io) сработал рейтлимит)
Укажем в настройках `Public Address` и `Lets Encrypt Domain` адрес bastion хоста:

```bash
echo vpn.$(curl 2ip.ru --silent | sed -e's/\./-/g').sslip.io
```

Теперь админ. панель доступна по адресу с валидным сертификатом:
[https://vpn.62-84-115-123.sslip.io](https://vpn.62-84-115-123.sslip.io)

---
# robots.txt
```
bastion_IP = 62.84.115.123
someinternalhost_IP = 10.128.0.3
```
