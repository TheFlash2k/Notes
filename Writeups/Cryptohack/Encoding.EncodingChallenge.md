The following python script extracts the flag:

```python
from ciphers import Encodings
from socket import *

types = ['bigint', 'utf-8', 'hex', 'base64', 'rot13']

def extract_data(data):
	data = data.decode().split(":")
	encoding = data[1]
	encoding = encoding.strip().replace('"', '').split(',')[0]
	try:
		encoded_data = data[2].strip().split('"')[1]
	except IndexError:
		encoded_data = data[2].strip().replace('}', '').replace('[', '').replace(']', '').replace(',', '')
		encoded_data = [int(item) for item in encoded_data.split()]
	return encoding, encoded_data

if __name__ == "__main__":

	s = socket()
	s.connect(("socket.cryptohack.org", 13377))

	for i in range(100):
		encoding, encoded_data = extract_data(s.recv(1024))

		print(f"[+] Encoding : {encoding} -> Encoded: {encoded_data} ", end='')
		decoded = ""
		if encoding == 'rot13':
			decoded = Encodings.Rot13.decode(encoded_data)
		elif encoding == 'bigint':
			decoded = Encodings.To_Ascii.big_int_to_ascii(encoded_data)
		elif encoding ==  'utf-8':
			decoded = Encodings.To_Ascii.dec_to_ascii(encoded_data)
		elif encoding == 'base64':
			decoded = Encodings.Base64.decode(encoded_data)
		elif encoding == 'hex':
			decoded = Encodings.To_Ascii.hex_to_ascii(encoded_data)

		print(f"-> Decoded: {decoded}")
		json_send = "{" + f'"decoded" : "{decoded}"' + "}"
		s.send(json_send.encode())
	flag = s.recv(1024).decode().split('"')[3]
	print(f"Flag is : {flag}")

```

I wrote a python script called ciphers so I can easily encode and decode stuff, it is in my root/scripts/ciphers.py