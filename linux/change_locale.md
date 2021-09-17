to disable warning of elixir (iex, mix, etc.):
```
warning: the VM is running with native name encoding of latin1 which may cause Elixir to malfunction as it expects utf8.
```

use `locale` check OS current locale setting.
use `locale -a` check OS supported locale.  

if current locale is `en_US.UTF-8` and support `C.UTF-8`, then change LANG to `C.UTF-8` by editing /etc/default/locale:
```
LANG=C.UTF-8
```
