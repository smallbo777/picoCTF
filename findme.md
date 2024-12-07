# findme
Description:Help us test the form by submiting the username as test and password as test!

#### Solution:
use the given username and password login

there only have search box in the page 

try to seacrh somthing but don't have any change 

so, back to the login page

![image](https://hackmd.io/_uploads/BkM0RoZVkg.png)

use burp suite intercept the page and enter login information

then forward it, it will have a get request /next-page and a id look like a base64 encoded

save id and keep forwarding 

a /next-page request is showed and have id look like a base64 encoded too

combine two ids and decode them 

the flag is found :)


