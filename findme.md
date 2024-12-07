# findme
Description:Help us test the form by submiting the username as test and password as test!

#### Solution:
use the given username and password login

there only have search box in the page 

try to seacrh somthing but don't have any change 

so, back to the login page

![image](https://github.com/user-attachments/assets/5abb2a7b-aeb9-4851-9ed0-e29cf60078ed)

use burp suite intercept the page and enter login information

then forward it, it will have a get request /next-page and a id look like a base64 encoded

save id and keep forwarding 

a /next-page request is showed and have id look like a base64 encoded too

combine two ids and decode them 

the flag is found :)


