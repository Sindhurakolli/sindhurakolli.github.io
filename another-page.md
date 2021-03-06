
# THE DATABASE

Description: This is a report about the database work done in the project

The primary task is to design the conceptual schema and create a physical database which stores the images with the required attributes that are generated using the MAYA software.

Initially, a python script was created to get just one image property using OpenCV imported as cv2. . Anyone can look up for any 3D image shots captured using Maya from the database.

OpenCV (Open Source Computer Vision Library: http://opencv.org) is an open-source BSD-licensed library that includes several hundreds of computer vision algorithms.This package is used to extract all the image properties.

The code below is the snippet to grab the image properties of one image.

```python
import numpy as np 
import cv2
cv2.imread('.../Desktop/Capture.png')
print('Image shape is \n', img.shape)
print('Image shape is \n', img.size)
print('Image datatype is \n', img.dtype)
```

The next step to get the data of the images is to loop all the images in a folder and grab the image properties to store it to .CSV
One main constraint is the way how the images are stored in the csv. Initially the idea was to save the image path in the database which dose not complete the requirement of creating an image database. To overcome this, the images are stored in BLOB (Binary Large objects).
BLOB is a collection of binary data which is generally of an image or an audio file wchich is stored as a single entity in a database system. 

```python
import pandas as pd
import numpy as np
import cv2
import os
import os, os.path
photos = []
for filename in os.listdir('.../Desktop/photos'):
   photos.append(filename)
file_name = []
shape = []
size =[]
img_type = []
for i in range(0,len(photos)):
   fn = '.../Desktop/photos/{}'.format(photos[i])
   file_name.append(fn)
for i in file_name:
   img = cv2.imread(i)
   shape.append(img.shape)
   size.append(img.size)
   img_type.append(img.dtype)
   image_df = pd.DataFrame(np.column_stack([photos,shape,size,img_type]), 
   columns= ['file_name','shape_1','shape_2','shape_3','size','type'])
image_df.to_csv (r'.../Desktop/image.csv', index = None, header=True)

```
The below code snippet is used to convert the image file into blob.

``` python
import sys, os

def convertToBinaryData(filename):
    with open(filename, 'rb') as file:
        binaryData = file.read()
    return binaryData

direcotry_name = os.path.dirname(sys.argv[0])
l = []                          ## initialize few lists which we will later use
oid = 0                         #.
img_id = []                     #.
cat_img_name =[]                #.
img_type = []                   #.
img_res = []                    #.
img_name = []                   #.
img_size = []                   #.
blob_data = []                  #.
x = []                          #.
y = []                          #.
z = []                          ##

img_res_val = "960x540"
img_type_val  = "png"
for subdir, dirs, files in os.walk(direcotry_name):
    for file in files:
        filepath = subdir + os.sep + file
        cat_img = filepath.replace(subdir, "")
        cat_img_name.append(cat_img)
        img_res.append(img_res_val)
        img_type.append(img_type_val)
        img_name.append(cat_img.replace(".png",""))
        img_size.append(os.path.getsize(filepath))
        blob_data.append(convertToBinaryData(filepath))
        e = re.findall("X\d+.", filepath)                       # find the x degree using regex
        qewg=e[0].replace("X","").replace("_", "")
        x.append(qewg)
        e = re.findall("Y\d+.", filepath)                       # find the y degree using regex
        qewg=e[0].replace("Y","").replace("_", "")
        y.append(qewg)
        e = re.findall("Z\d+.", filepath)                       # find the z degree using regex
        qewg=e[0].replace("Z","").replace("_", "")
        z.append(qewg)
        oid = oid + 1
        img_id.append(oid)
data = {
    "image_id"  : img_id,
    "image_name": img_name,
    "image_size": img_size,
    "image_reso": img_res,                              # create a data dictonary with all the lists
    "image_type": img_type,
    "image_BLOB": blob_data,
    "X"         : x,
    "Y"         : y,
    "Z"         : z,

}
import pandas as pd
dataFrame = pd.DataFrame(data)                                  # convert data dictonary into a dataframe

dataFrame.to_csv('final.csv',encoding='utf-8')                  # export the dataFrame into a csv

```

The final result is stored into a CSV which is later imported into the database schema.

#### Conceptual Schema


Below is the initial conceptual database schema:


![Octocat](https://raw.githubusercontent.com/Sindhurakolli/DMDD_portfolio/harini/hiii.png)

Later, we came up with few modifications to the schema to add more attributes related to the image and optimise the design.

![Octocat](https://raw.githubusercontent.com/Sindhurakolli/DMDD_portfolio/master/Final_DB_Schema.png)


The above schema can be explained with an example below:


<br> category - table 
<br> category_id: C01 (PK) 
<br> category_name: Animals 


<br> sub_category - table
<br> sub_category_id: C01_01 (PK)
<br> category_id: C01(FK)
<br> sub_category_name: Dog


<br> object - table
<br> object_id: O1_01 (PK)
<br> object_name: labrador
<br> sub_category_id: C01_01 (FK)
<br> texture_id : tx_01
<br> background_id : bk_01
<br> shadow_id : sh_01


<br> texture - table
<br> texture_id : tx_01 (PK)
<br> texture_name : black


<br> background - table
<br> background_id : bk_01 (PK)
<br> background_name : white


<br> shadow - table
<br> shadow_id : sh_01 (PK)
<br> shadow_presence : yes


<br> object_image_junction - table
<br> image_id : I01
<br> object_id : O1_01


<br> image_data - TABLE
<br> image_id: I01 (PK)
<br> image_name : (FK)
<br> image_size: in KB
<br> image_resolution: 960X540
<br> image_type: .png
<br> image: BLOB image
<br> X: angle in degree
<br> Y: angle in degree
<br> Z: angle in degree


A physical database on MYSQL Workbench was created.

To establish a connection with the MYSql workbench from python, the following script has to be run to create a connection. This will enable the user to directly connect to the database environment and import the files created directly into the database. 

Code snippet for connecting the database is as follows

```sql
ALTER USER 'root'@'localhost'
 IDENTIFIED WITH mysql_native_password
 BY 'password';
```

```python
import mysql.connector
from mysql.connector import errorcode
try:
  cnx = mysql.connector.connect(host= 'localhost',
                                user='root',
                                password='password',
                                auth_plugin='mysql_native_password',
                                database='images')
  if cnx.is_connected():
            print('Connected to MySQL database')
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with your user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cnx.close()

```
Challenge faced during the connection establishment is regarding the password authentication with the local host for which the user has to be identified with mysql_native_password on workbench using the below query.

# Few of the Usecases

1)Search by category and display subcategories 
```sql
SELECT s.subcategory_name,s.subcategory_id from sub_category s, category c where s.category_id = c.category_id and c.category_name like 'car'
```

2)Search by subcategories and display objects 
```sql
SELECT o.object_id, o.object_name from object o, sub_category s where s.subcategory_id = o.subcategory_id and s.subcategory_name like 'audi'
```

3)Search by object and display all it's images 
```sql
SELECT i.* from image i, object o, object_image_junction j where i.image_id = j.image_id and o.object_id = j.object_id and o.object_name like 'AudiA4'
```

4)Search by angle and display images 
```sql
SELECT * FROM image where x = 0 Or y = 0 or z = 100
```

5)Search images of a particular object by it's texture
```sql
SELECT i.* from image i, object o, object_image_junction j, texture t where i.image_id = j.image_id and o.object_id = j.object_id and t.texture_id = o.texture_id and t.texture_name like 'white' and o.object_name like 'AudiA4'
```



Contibutions towards the Database Schema, Physical database, Use cases creation:
<br> Harini Grandhi
<br> Sindhura Kolli
<br> Anindita Baishya
<br> Rashika Moza


Please click on below link to go to the cloud details.

* * *

[The Cloud](./cloud.html).


* * *


[Back](./)
