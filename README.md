# Demo: Working with Workspace

## Examples

### Simple command

```yaml
command('say-hello'): |
  #!bash
  echo 'Hello World'
```

### Using attributes

```yaml
attribute('message'): Hello World!

command('say-hello'): |
  #!bash|@
  echo @('message')
```

### Using arguments

```yaml
command('say-hello <name>'): |
  #!bash|=
  echo ={ @('message') } from ={ input.argument('name') }
```

### Using arguments with envrionment variables

```yaml
command('say-hello <name>'):
  env:
    MESSAGE: = @('message')
    NAME: = input.argument('name')
  exec: |
    #!bash|=
    echo "$MESSAGE from $NAME"
```

### Managing secrets

```yaml
# ws secret generate-random-key 
key('default'): 'd38be3b7aa42fdbfb14c0d25f07bc1875edd5f13f640cd7d9e3bd7f67b3c3716'

# ws secret encrypt 'Hello World!'
attribute('message'): = decrypt('YTozOntpOjA7czo3OiJkZWZhdWx0IjtpOjE7czoyNDoidJHP5MrjXU+iRjfzCnb+pQxjO+fudniUIjtpOjI7czoyODoi6kP/Zq8a3fzSMK/tC8XToLgJ9yPY8hNAe4BO3iI7fQ==')

command('say-hello'): |
  #!bash|@
  echo @('message')
```

### Configuration files

```yaml
command('apply config'): |
  #!php
  $ws->confd('workspace:/confd')->apply();

confd('workspace:/confd'):
  - { src: 'settings.php', dst: 'workspace:/settings.php' }

attributes:
  database:
    driver: mysql
    host: mysql
    name: drupal
    username: user
    password: = decrypt('YTozOntpOjA7czo3OiJkZWZhdWx0IjtpOjE7czoyNDoi6LHweHpBsrOBtVHCFO/L4nuMnxuCejIiIjtpOjI7czoyMjoirG3KlBnw952zVvLWUSmdPAqJsql/vCI7fQ==')
```
