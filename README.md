# BigBlueButton
Ansible role for a bigbluebutton installation (following the documentation on http://docs.bigbluebutton.org/install/install.html)

## Role Variables
| Variable Name | Function | Default value | Comment |
| ------------- | -------- | ------------- | ------- |
| `bbb_hostname` | Hostname for this BigBlueButton instance _(required)_ | {{ ansible_fqdn_hostname }} |
| `bbb_letsencrypt_enable` | Enable letsencrypt/HTTPS | `yes` |
| `bbb_letsencrypt_email` | E-mail for use with letsencrypt _(required when using LE)_|  |
| `bbb_coturn_enable` | enable installation of the TURN-server | `yes` |
| `bbb_turn_enable` | enable the use uf TURN in general | `yes` |
| `bbb_turn_server` | the adress for the TURN-Server to use | `{{ bbb_hostname }}` | has to be a fully qualified domain name
| `bbb_turn_secret` | Secret for the TURN-Server  _(required)_ | | can be generated with `openssl rand -hex 16`
| `bbb_additional_turn_server` | the address for an additional TURN-Server to use | | has to be a fully qualified domain name. also needed: `bbb_additional_turn_secret`
| `bbb_additional_turn_secret` | secret for the additional TURN-Server if defined | | only needed, if `bbb_additional_turn_server` is defined
| `bbb_greenlight_enable` | enable installation of the greenlight client | `yes` |
| `bbb_greenlight_secret` | Secret for greenlight _(required when using greenlight)_ |  | can be generated with `openssl rand -hex 64`
| `bbb_greenlight_db_password` | Password for greenlight's database  _(required when using greenlight)_ | | can be generated with `openssl rand -hex 16`
| `bbb_api_demos_enable` | enable installation of the api demos | `no` |
| `bbb_nodejs_version` | version of nodejs to be installed | `8.x` |
| `bbb_system_locale` | the system locale to use | `en_US.UTF-8` |

### Extra options for Greenlight
The Web-Frontend has some extra configuration options, listed below:
#### SMTP
The notifications are sent using sendmail, unless the `bbb_greenlight_smtp.server` variable is set.
In that case, make sure the rest of the variables are properly set.

The default value for `bbb_greenlight_smtp.sender` is `bbb@{{ bbb_hostname }}`

Example Setup:
```yaml
bbb_greenlight_smtp:
  server: smtp.gmail.com
  port: 587
  domain: gmail.com
  username: youremail@gmail.com
  password: yourpassword
  auth: plain
  starttls_auto: true
  sender: youremail@gmail.com
```

#### LDAP
You can enable LDAP authentication by providing values for the variables below.
Configuring LDAP authentication will take precedence over all other providers.
For information about setting up LDAP, see: https://docs.bigbluebutton.org/greenlight/gl-config.html#ldap-auth

Example Setup:
```yaml
bbb_greenlight_ldap:
  server: ldap.example.com
  port: 389
  method: plain
  uid: uid
  base: dc=example,dc=com
  bind_dn: cn=admin,dc=example,dc=com
  password: password
  role_field: ou
```

#### GOOGLE_OAUTH2
For in-depth steps on setting up a Google Login Provider, see:  https://docs.bigbluebutton.org/greenlight/gl-config.html#google-oauth2
The `bbb_greenlight_google_oauth2.hd` variable is used to limit sign-ins to a particular set of Google Apps hosted domains. This can be a string with separating commas such as, 'domain.com, example.com' or a string that specifies a single domain restriction such as, 'domain.com'. If left blank, GreenLight will allow sign-in from all Google Apps hosted domains.

```yaml
bbb_greenlight_google_oauth2:
  id:
  secret:
  hd:
```

#### OFFICE365
For in-depth steps on setting up a Office 365 Login Provider, see: https://docs.bigbluebutton.org/greenlight/gl-config.html#office365-oauth2

```yaml
bbb_greenlight_office365:
    id:
    secret:
    hd:
```

#### RECAPTCHA
To enable reCaptcha on the user sign up, define these 2 keys.
You can obtain these keys by registering your domain using the following url: https://www.google.com/recaptcha/admin

```yaml
bbb_greenlight_recaptcha:
  site_key:
  secret_key:
```

## Dependencies
- [geerlingguy.nodejs](https://github.com/geerlingguy/ansible-role-nodejs)
- [geerlingguy.docker](https://github.com/geerlingguy/ansible-role-docker)

## Example Playbook
This is an example, of how to use this role. Warning: the value of the Variables should be changed!

    - hosts: servers
      roles:
         - { role: n0emis.bigbluebutton, bbb_turn_secret: ee8d093109a9b273, bbb_greenlight_secret: 107308d54ff4a5f, bbb_greenlight_db_password: 2585c27c785e8895ec, bbb_letsencrypt_email: mail@example.com }

## License
MIT
