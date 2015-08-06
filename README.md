#Royal Android
**This project created to help developer to have better experience in using [Realm-Java](https://github.com/realm/realm-java)**

Realm is very fast database, but unfortunately there is some usability issues.

1. `Realm`, `RealmObject` can not be accessed by different thread other then which is was first created.
2. You have to `realm.getInstance()` and `realm.close()` every time you use Realm.
3. You have to call `realm.beginTransaction()` and `realm.commitTransaction()` every time you want to save data.
4. You have to loop for-loop backward `for(int i = results.size() - 1; i >= 0; i--)` to update data inside it.
5. Very hard to use Realm browser since Android device doesn't give you permission to pull the "name.realm" file unless you have rooted the phone.

###Dependencies
```java
dependencies {
   compile 'com.thefinestartist:royal:0.82.0.6'
}
```


###Royal
```java
public class App extends Application {

   @Override
   public void onCreate() {
      super.onCreate();
      Royal.joinWith(this);
   }
}
```

###Royal Database
```java
public class SecondaryDatabase extends RoyalDatabase {

   public String getFileName() {
      return "secondary";
   }

   public boolean forCache() {
      return false;
   }

   public byte[] getEncryptionKey() {
      return null;
   }

   public int getVersion() {
      return 0;
   }

   public boolean shouldDeleteIfMigrationNeeded() {
      return false;
   }

   public Set<Object> getModules() {
      Set<Object> set = new HashSet<>();
      set.add(new SecondaryModule());
      return set;
   }

   @Override
   public long execute(Realm realm, long version) {
      return getVersion();
   }

   @RealmModule(classes = {Dog.class, Cat.class, Owner.class})
   public static class SecondaryModule {
   }
}

public class App extends Application {

   @Override
   public void onCreate() {
      super.onCreate();
      Royal.joinWith(this);
      Royal.addDatabase(new SecondaryDatabase());
   }
}
```

```java
RealmConfiguration configuration = Royal.getConfigurationOf(SecondaryDatabase.class);
```

###RoyalTransaction
```java
// RealmList, RealmResults, RealmObject, List<? extends RealmObject>
RoyalTransaction.create(Class<? extends RoyalDatabase> clazz, Object... objects);
RoyalTransaction.create(RealmConfiguration configuration, Object... objects);
RoyalTransaction.create(Realm realm, Object... objects);

RoyalTransaction.save(Class<? extends RoyalDatabase> clazz, Object... objects);
RoyalTransaction.save(RealmConfiguration configuration, Object... objects);
RoyalTransaction.save(Realm realm, Object... objects);

RoyalTransaction.delete(Class<? extends RoyalDatabase> clazz, Object... objects);
RoyalTransaction.delete(RealmConfiguration configuration, Object... objects);
RoyalTransaction.delete(Realm realm, Object... objects);
```

```java
User user = new User();
user.setName("Leonardo Taehwan Kim");
user.setEmail("contact@thefinestartist.com");
user.setUsername("TheFinestArtist");

RoyalTransaction.save(realm, user);
```

```java
User user1 = new User();
user1.setName("Leonardo Taehwan Kim");
user1.setEmail("contact@thefinestartist.com");
user1.setUsername("TheFinestArtist");

User user2 = new User();
user2.setName("Leonardo Taehwan Kim");
user2.setEmail("contact@thefinestartist.com");
user2.setUsername("TheFinestArtist");

RoyalTransaction.save(realm, user1, user2);
```

###RoyalExport
```java
RoyalExport.toEmail(RealmConfiguration... configurations);
RoyalExport.toEmail(String email, RealmConfiguration... configurations);
RoyalExport.toEmail(String email, Intent intent, RealmConfiguration... configurations);

RoyalExport.toEmail(Realm... realms);
RoyalExport.toEmail(String email, Realm... realms);
RoyalExport.toEmail(String email, Intent intent, Realm... realms);

RoyalExport.toEmailAsRawFile();
RoyalExport.toEmailAsRawFile(String email);
RoyalExport.toEmailAsRawFile(String email, Intent intent);

RoyalExport.toExternalStorage(RealmConfiguration... configurations);
RoyalExport.toExternalStorage(Realm... realms);
RoyalExport.toExternalStorageAsRawFile();
```

###Rson
```java
Gson gson = Rson.getGson();

Rson.toJsonString(RealmObject object);
Rson.toJsonString(RealmObject object, int depth);
```


## License
```
The MIT License (MIT)

Copyright (c) 2015 TheFinestArtist

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
