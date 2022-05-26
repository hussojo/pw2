# pw2
pw2

I continue with my django development database (readme link here)

First I create an app called luetutkirjat (trying to do an app where people can add books they have read, and rewiev them shortly).

command to create an app:
```
(env) josefina@x230:~/hussojo$ ./manage.py startapp luetutkirjat
```
then I have to add this new app to settings.py:
```
(env) josefina@x230:~/hussojo$ micro hussojo/settings.py 
```


![Screenshot from 2022-05-26 18-34-35](https://user-images.githubusercontent.com/106306928/170542530-f367b3e6-9b49-45d8-8bff-9facfb5a7191.png)



Next I added a model to luetutkirjat/models.py


![Screenshot from 2022-05-26 18-38-04](https://user-images.githubusercontent.com/106306928/170542584-6a2458b5-59c4-47fd-a3ee-5d31fc145291.png)



Then I made migrations and tested if the app works
When migrating I got an error, because I made a typo in models.py
```
File "/home/josefina/hussojo/luetutkirjat/models.py", line 4
    name = models.CharField(max-length=300)
                           ^
SyntaxError: expression cannot contain assignment, perhaps you meant "=="?
```
Fixed the typo and migrated again

then in the luetutkirjat/admin.py I imported models and registered luetutkirjat app


![Screenshot from 2022-05-26 18-47-05](https://user-images.githubusercontent.com/106306928/170542740-ca0c7118-cb0a-4603-9aac-88494bdbb80d.png)



I ran the server and got an error
```
  File "/home/josefina/hussojo/luetutkirjat/admin.py", line 4, in <module>
    admin.site.register(models.luetutkirjat)
AttributeError: module 'luetutkirjat.models' has no attribute 'luetutkirjat'
```
 And I got this because I had named my class in models.py "Kirjat" not luetutkirjat. I fixed registration in admin.py and ran server


![Screenshot from 2022-05-26 18-52-51](https://user-images.githubusercontent.com/106306928/170542786-f5108579-a891-4126-b8ea-879150c17a91.png)


 
 And it worked!
 
 I added a few books but instead of seeing book titles listed I see this:
 
 
 ![Screenshot from 2022-05-26 18-56-08](https://user-images.githubusercontent.com/106306928/170542858-440bfdbb-e321-408c-90d0-98a8ef03bf72.png)


So now I have to make my class atribute "name" a function that returns books real tittle.

Before I added the function I wanted to change the class name "Kirjat" to "Book", so it would be consistent with other class names and attributes that are in english. Ofcourse my app name is still in Finnish, might change that later...

After changin the name in models.py and admin.py I run migrates. Django really is smart, it wanted to verify the change I made:
```
josefina@x230:~/hussojo$ ./manage.py makemigrations
Was the model luetutkirjat.Kirjat renamed to Book? [y/N] y
Migrations for 'luetutkirjat':
  luetutkirjat/migrations/0002_rename_kirjat_book.py
    - Rename model Kirjat to Book
josefina@x230:~/hussojo$ ./manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, luetutkirjat, sessions
Running migrations:
  Applying luetutkirjat.0002_rename_kirjat_book... OK
```
Now I was ready to continue with the name return function.
In models.py I added the function
```
    def __str__(self):
        return self.name
```       
Testing if it works...


![Screenshot 2022-05-26 at 20-32-02 Screenshot from 2022-05-26 19-06-41 png (PNG Image, 1366 × 663 pixels) — Scaled (51%)](https://user-images.githubusercontent.com/106306928/170543294-cac50924-ca3c-41ec-92a9-f272c74fc193.png)



..And it did!

Next I want to add more fields to my class, like release year, author and comment field.

I wanted to test as little as possible first (so I commented out everything else than year)


![Screenshot from 2022-05-26 19-23-18](https://user-images.githubusercontent.com/106306928/170543436-3046b5f8-e555-47f9-b3b6-ce0a27ea44ac.png)


I ran server and got a error message, because syntax was missing a comma:
```
  File "/home/josefina/hussojo/luetutkirjat/models.py", line 5
    year = models.CharField(max_length=10 default=0)
                                          ^
SyntaxError: invalid syntax
```
After fixing that I got another error


![Screenshot 2022-05-26 at 19-33-28 OperationalError at admin luetutkirjat book 3 change ](https://user-images.githubusercontent.com/106306928/170543487-4f68dfc5-5ab9-4573-9bf6-f2c5e06eac24.png)


I ran migrations and tried again..

Now I was able to add year for each book. 

Next I'm trying to add other fields, and I got this
```
(env) josefina@x230:~/hussojo$ ./manage.py makemigrations
It is impossible to add a non-nullable field 'author' to book without specifying a default. This is because the database needs something to populate existing rows.
```
With this guide https://www.pythonfixing.com/2021/10/fixed-you-are-trying-to-add-non.html I fixed the problem adding null=True
```
 author = models.CharField(max_length=300, null=True)
```
 Now I was abble to add rest of the fields.
 
 
![Screenshot 2022-05-26 at 20-05-14 An Idiot Abroad Change book Django site admin](https://user-images.githubusercontent.com/106306928/170543633-346ab43c-5557-4dee-8b2f-fe7919f8daf2.png)
