# HTB Toxic Walkthrough By Tirex
![image](https://user-images.githubusercontent.com/31727214/231840187-0cbb40a8-82d4-49bc-96ae-4acee4cf4765.png)

This [Room](https://app.hackthebox.com/challenges/Toxic) will help you to learn about session hijacking and escalating the LFI to RCE vulnerability

## Discovering

This Room comes with Source Code files

![image](https://user-images.githubusercontent.com/31727214/231865140-3deedf7b-d279-4403-b275-c7110838d596.png)
 
![image](https://user-images.githubusercontent.com/31727214/231866075-9190aabe-cdc9-4747-a73f-c47fe2a8bad4.png)

Inside challenge Folder we can Find `PHP File` that contain our Key solution to solve this room 

![image](https://user-images.githubusercontent.com/31727214/231866507-0b520fa1-c813-40c7-93ab-cf19efe6033e.png)

1 - Using `Nmap`

`nmap -sC -sV -p Port -Pn Ip `

![image](https://user-images.githubusercontent.com/31727214/231867713-33960efc-ce0e-4e81-a561-2d1faec55cf7.png)

We found
```ngnix Server```

```HttpOnly Flag Not Set``` [HttpOnly OWASP](https://owasp.org/www-community/HttpOnly)

Now lets check the files we start with `index.php`

![image](https://user-images.githubusercontent.com/31727214/231871018-3ea08790-680d-4776-af33-2bf0be47e978.png)

we notice that PHPSESSID cookie value is base64 encoded and it shows the webpage. So the default Value will point automatically to /www/index.html
And to Verify this point we can easily decode the cookie value with [base64 Decoder](https://www.base64decode.org/) 

![image](https://user-images.githubusercontent.com/31727214/231872351-002cde8e-218c-4c07-b1b7-f2478ed5bb6f.png)

![image](https://user-images.githubusercontent.com/31727214/231872549-f8e2b776-5884-41ba-ada4-f0905a8f029b.png)

So Cookie Value after Decoding give to us this Value : `O:9:"PageModel":1:{s:4:"file";s:15:"/www/index.html";}`

`s:` refers to character numbers between " "
so our point is proved and verified by decoding the cookie value

After all this Infos that we have the idea is hijacking via malicious cookies to discover Some Data from server side.

![image](https://user-images.githubusercontent.com/31727214/231878821-7667dc22-9c5d-4e22-a9f7-3dc6b801d6d8.png)

Nmap show that used server is nginx. After some Docs review we found [NGINX access log](https://www.digitalocean.com/community/tutorials/nginx-access-logs-error-logs) Default path `/var/log/nginx/access.log;`

![image](https://user-images.githubusercontent.com/31727214/231879538-37811838-e95f-411a-971b-88381b36674b.png)

lets try to make Malicious Cookie value to this default path and we see if the Dev Team change it or no 

![image](https://user-images.githubusercontent.com/31727214/231880941-420986b1-4102-4051-a51e-6ede032beddc.png)

We inject the New Cookie Value to the WebPage

![image](https://user-images.githubusercontent.com/31727214/231881388-ac03b8de-62c9-4193-ae1b-17965e30af7e.png)

Now everything will be easy The Dev Team did not change the default path so we can inject some shell codes Via User Agent

for that you can use Exploit.py file that i create to inject and this is the results 

![image](https://user-images.githubusercontent.com/31727214/231882989-a8d6f941-def4-4f00-8eb3-4ca3c5a5a59b.png)





