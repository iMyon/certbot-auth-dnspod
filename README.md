# Certbot DNS Authenticator For DNSPod
fork from [al-one/certbot-auth-dnspod](https://github.com/al-one/certbot-auth-dnspod), but for dnspod.com.

**只适用于dnspod.com**，dnspod.cn请用原版[al-one/certbot-auth-dnspod](https://github.com/al-one/certbot-auth-dnspod)

此脚本针对域名使用DNSPod解析，且certbot使用dns方式验证。如果在执行`certbot renew`时出现如下错误时，可以试试此脚本。
> Could not choose appropriate plugin: The manual plugin is not working; there may be problems with your existing configuration. The error was: PluginError('An authentication script must be provided with --manual-auth-hook when using the manual plugin non-interactively.',). Skipping.


## Installing

```sh
$ wget https://raw.githubusercontent.com/iMyon/certbot-auth-dnspod/master/certbot-auth-dnspod.sh
$ chmod +x certbot-auth-dnspod.sh
```

## Config

> Get Your DNSPod Token From https://console.dnspod.com/account/token/token

> Token Format: `ID,Token` See: https://docs.dnspod.com/api/5fe199ea6e336701a2111ba3/

```sh
$ export DNSPOD_TOKEN="your dnspod token"
```

or

```sh
$ echo "your dnspod token" > /etc/dnspod_token
```

or

```sh
$ echo "your dnspod token" > /etc/dnspod_token_$CERTBOT_DOMAIN
# echo "your dnspod token" > /etc/dnspod_token_laravel.run
```


## Usage

```sh
$ certbot certonly --manual --preferred-challenges dns-01 --email mail@domain.com -d laravel.run -d *.laravel.run --server https://acme-v02.api.letsencrypt.org/directory --manual-auth-hook /path/to/certbot-auth-dnspod.sh --manual-cleanup-hook "/path/to/certbot-auth-dnspod.sh clean"
```

or

```sh
$ certbot renew --manual-auth-hook /path/to/certbot-auth-dnspod.sh --manual-cleanup-hook "/path/to/certbot-auth-dnspod.sh clean"
```

or add crontab

```crontab
0 2 1 * * sh -c 'date "+\%Y-\%m-\%d \%H:\%M:\%S" && /usr/bin/certbot renew --manual-auth-hook /path/to/certbot-auth-dnspod.sh --manual-cleanup-hook "/path/to/certbot-auth-dnspod.sh clean"' >> /var/log/certbot-renew.log 2>&1
```
