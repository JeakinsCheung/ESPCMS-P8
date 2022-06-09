**Arbitrary code execution vulnerability exists in ESPCMS management system**

 



**Vulnerability description:**

The vulnerability modifies the content of the homepage template file in the background, and after modification, a PHP suffix file with the same content will be generated. When the frontend accesses the homepage file, local code execution will be triggered.

**Supplier:** https://www.ecisp.cn/

**Vulnerability file:** 

espcms\espcms_public\espcms_templates\ESPCMS_Templates.php

**Code Analysis:**

The code execution function eval is called in line 165. The content obtained by the $out variable is the content of the template file. The $fetch_filename parameter in line 84 is actually the address of the template file. In line 90, it is simply obtained with the file_get_contents() function. The contents of the template file are then assigned to $out.

espcms\espcms_public\espcms_templates\ESPCMS_Templates.php


![image-20220609095741829](https://user-images.githubusercontent.com/57223351/172748831-b544123a-cf2a-43ff-ab27-13d9914ba484.png)


![image-20220609095754349](https://user-images.githubusercontent.com/57223351/172748855-3d96fd56-3523-490b-8360-8b51ca80f245.png)

This function is a function to modify the content of the template file. There are user-controllable input parameters in line 174, and the content is written to the template file in line 211.

espcms\espcms_admin\control\TemplateFile.php


![image-20220609095824468](https://user-images.githubusercontent.com/57223351/172748870-8262c941-1afa-4766-b921-b67302238f3d.png)



![image-20220609095828929](https://user-images.githubusercontent.com/57223351/172748888-e3618f5b-bfa2-434d-b1dc-8797157411d6.png)


**Steps to reproduce:**

\1. Log in to the background management page as an administrator

\2. Click Template Management -> Modify and change the content to <?php phpinfo();?>


![image-20220609095846409](https://user-images.githubusercontent.com/57223351/172748901-e6d92344-5fd3-4c0d-bba2-86e46497d587.png)



![image-20220609095852932](https://user-images.githubusercontent.com/57223351/172748920-0fb6bdc9-39e0-4a69-bb3f-e31b066457b1.png)


\3. After the modification is successful, save it, and access the home page to cause the code to execute.


![image-20220609095859737](https://user-images.githubusercontent.com/57223351/172748942-f2ba9e45-6a64-44af-babe-fdd3cf669068.png)
