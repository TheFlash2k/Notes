# Natas 0:
Simply checking the page source will give us the flag.

# Natas 1:
Right clicking has been block so simply pressing CTRL + U to get the source code of the page.

# Natas 2:
We get a prompt that there is nothing on this page so we see the source. In source we see a picture being imported `files/pixel.png`. We open that file and don't see anything so we try the folder `files/`. In there, we see a users.txt, in that, we get the password for the next stage.

# Natas 3:
Once again, we get the prompt that there is nothing on this page so again we see the source. In source we get a comment :
```html
<!-- No more information leaks!! Not even Google will find it this time... -->
```
So, that hints us to go check the file `robots.txt`. So we open that file and we see a folder called `/s3cr3t/` and then when we visit that folder, we see a file named `user.txt` and inside that, we can find our password.

# Natas 4:
Well this challenge was a bit more complicated than the other. I've done some CTFs in the past and that's why I knew that I had to add the `referer` http tag to be set to `natas5`. So, I fired up Burp Suite (you can also do this in python using requests or even with curl). Then, refreshed the page and changed the referer tag to `natas5` and we got the password.
Using curl:
```bash
curl -u natas4:Z9tkRkWmpt9Qr7XrR5jWRkgOU901swEZ http://natas4.natas.labs.overthewire.org/index.php --header "referer: http://natas5.natas.labs.overthewire.org/" | grep -i pass | cut -d ' ' -f 8 | tr '\n' ' ' | cut -d ' ' -f 2
```

# Natas 5:
This challenge involved changing the cookie. Initially I did not know what to do so I just simply intercepted the request in Burp when refreshing the page and saw a `loggedIn=0` cookie. I just changed that to `1` and got the flag.

# Natas 6:
We see a simple prompt to `input secret` and a button `Submit Query`. We also get another button to `View Sourcecode`. We check that the url of this page and the page that the button `View sourcecode` will open are different. So, we firstly check the source code of this page and don't find anything useful so we click on the hyperlink. We're redirected to `/index-source.html`. Now, when we're redirected, we see some php code that looks like some sort of verification that might be happening on the input that we provide.
```php
<?  
include "includes/secret.inc";  
  
    if(array_key_exists("submit", $_POST)) {  
        if($secret == $_POST['secret']) {  
        	print "Access granted. The password for natas7 is <censored>";  
    	} else {  
        	print "Wrong secret";  
    	}  
    }
?>  
```
So, we can see that `$secret` is a variable that contains the data with which our input will be verified. We can see a file `includes/secret.inc` also being included. We visit that website and we see an empty page. So, we check the source code and we see the following php code:
```php
<?
$secret = "FOEIUWGHFEEUHOFUOIU";
?>
```
So, we will now submit this to the input field that was provided to us and then we will get the password for the next level.

# Natas 7:
We can see two hyperlinks in the page. `Home` and `About`. If we were to read the url, we see that the pages are being included based on a `'GET'` variable `page` being set. So, from this, we can identify this as being a Local File Inclusion challenge. But exact file are we supposed to read? For that, we check the source code for a hint. There, we can find: 
```html
<!-- hint: password for webuser natas8 is in /etc/natas_webpass/natas8 -->
```
So, the next thing is to identify in which url we are. So, as an error. I simply provide `../` as an input to the page variable.
`http://natas7.natas.labs.overthewire.org/index.php?page=../`
We're greeted with a warning:
```
**Warning**: include(/var/www/natas): failed to open stream: No such file or directory in **/var/www/natas/natas7/index.php** on line **21**  
  
**Warning**: include(): Failed opening '../' for inclusion (include_path='.:/usr/share/php:/usr/share/pear') in **/var/www/natas/natas7/index.php** on line **21**
```
Now, we know that we're in `/var/www/natas/natas7/` directory. To traverse the directory to `/etc`. We need to climb up 4 directories i.e. `../` will take us to `/var/www/natas/`, then `../../` will take us to `/var/www/` then `../../../` will take us to `/var/` and then `../../../../` will take us to `/`. From there, we can simply goto `/etc/natas_webpass/natas8` to get the password. The final payload looks like this:
```
../../../../etc/natas_webpass/natas8
```
We visit the site like this to get the password for level 8:
```txt
http://natas7.natas.labs.overthewire.org/index.php?page=../../../../etc/natas_webpass/natas8
```

# Natas 8:
This one required some reverse engineering skills in order to reverse an encoding algorithm and then doing some step by step analysis as to what was happening as I do not know PHP. So, we simply start off and see a `View sourcecode` button so we move directly onto that. Now, once we go there we're greeted with an html page with some embedded php.
```php
<?    
$encodedSecret = "3d3d516343746d4d6d6c315669563362";  
  
function encodeSecret($secret) {  
    return bin2hex(strrev(base64_encode($secret)));  
}  
if(array_key_exists("submit", $_POST)) {  
    if(encodeSecret($_POST['secret']) == $encodedSecret) {  
    print "Access granted. The password for natas9 is <censored>";  
    } else {  
    print "Wrong secret";  
    }  
}
```
The main two things were pretty important. The `$encodedSecret` and the `encodeSecret` function. So, I firstly copied the `$encodedSecret` into a python script in order to reverse engineer the encoding algorithm. So, one thing I didn't understand was what exactly did bin2hex did. Just seeing that inside the `bin2hex()` function call, the parameter passed is the output of another function `strrev()` and into that function is passed another function's output `base64_encode()`. I knew I had to start from the outer edge and reverse it. But I must firstly understand encoding in order to decode it. So, from what I understood, the parameter passed `$secret` in our case, was firstly base64 encoded, then reversed and then the ascii representation of that specific string was then converted to its equivalent binary and then converted to hex. So, I actually did some manual php sandbox tinkering in order to understand this. Then, I started writing my own script in python. I knew I had to use hex2bin instead of bin2hex. So, instead of directly converting it to binary, i remembered that the actual algorithm was converting the ascii to binary and then to hex, so I just removed the intermediary step and converted hex directly to ascii using the following code:
```python
encoded_string = "3d3d516343746d4d6d6c315669563362"
bytes_obj = bytes.fromhex(encoded_string)
string = bytes_obj.decode("ASCII")
```

Now, the next step was to actually reverse the string, it's easy in python, so the following one line did everything:
```python
string = string[::-1]
```

So, now remains the base64 decoding, so in python, we can do that using `base64` package that's built in. So I imported that and simply decoded the string and we got the `$secret` that we entered into the website and we got the password for natas9. The full script is:

```python
#!/usr/bin/env python3

from base64 import b64decode

# hex2bin (hex2ascii actually xd)
encoded_string = "3d3d516343746d4d6d6c315669563362"
bytes_obj = bytes.fromhex(encoded_string)
string = bytes_obj.decode("ASCII")

# strrev
string = string[::-1]

# base64_decode
string = string.encode()
string = b64decode(string).decode()

print(string)
```

# Natas 9:
Once again, we're presented with a page and a `View Sourecode` button. The following php code is provided to us:
```php
<?  
$key = "";  
if(array_key_exists("needle", $_REQUEST)) {  
    $key = $_REQUEST["needle"];  
}    
if($key != "") {  
    passthru("grep -i $key dictionary.txt");  
}  
?>
```
Now, we can see that our input is being passed directly into a local command grep that will be executed. So, we simply pass in a `;` to break that command and then `ls` to check if we get any output:
```bash
; ls
```
Yes, we get the output : `dictionary.txt`. Now, we know that we have RCE (Remote Code Execution). From a previous task, we know that passwords for level are stored at : `/etc/natas_webpass/`. Since we're seeking password for natas 10, we try the following payload:
```bash
; cat /etc/natas_webpass/natas10 #
```
The `#` at the end will prevent the rest of the grep command from being executed and thus won't pollute our browser. So, when we send this payload, we get our password.

# Natas 10:
What we can do here is just simply provide . as an argument and read the password from the file. So, the payload would look something like this:
```bash
. /etc/natas_webpass/natas11 #
```