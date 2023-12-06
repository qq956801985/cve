Tongda OA has a front-end SQL injection vulnerability

version:Tongda<=v11.10、=v2017 version

1.
Route: general/vehicle/checkup/delete_search.php

There are injected parameters: $VU_ID

The code here is very concise. When $VU_ID is not empty, the parameters are directly spliced ​​into the SQL statement. Since the parentheses are closed here, there is a bypass.

![image](https://github.com/qq956801985/cve/assets/29126012/a6dfd527-37d4-4d40-b8c0-db1099985afb)
2.Payload
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/qq956801985/cve/assets/29126012/918f73f8-0510-470b-b25d-2e4310d11882)

And when we change 116 to 115, there is no delay here, indicating that there is SQL injection.

![image](https://github.com/qq956801985/cve/assets/29126012/7b8483bf-4c26-438e-9dd2-051e352decca)
