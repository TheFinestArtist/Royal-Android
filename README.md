#Royal Android
**This project created to help developer to have better experience in using [Realm-Java](https://github.com/realm/realm-java)**

Realm is very fast database, but unfortunately there is some usability issues.

1. `Realm`, `RealmObject` can not be accessed by different thread other then which is was first created.
2. You have to `realm.getInstance()` and `realm.close()` every time you use Realm.
3. You have to call `realm.beginTransaction()` and `realm.commitTransaction()` every time you want to save data.
4. You have to loop for-loop backward `for(int i = results.size() - 1; i >= 0; i--)` to update data inside it.
5. Very hard to use Realm browser since Android device doesn't give you permission to pull the "name.realm" file unless you have rooted the phone.

###RoyalTransaction

**API**
```java
// RealmList, RealmResults
RoyalTransaction.save(Realm, RealmObject...);
RoyalTransaction.save(Transaction, Realm, RealmObject...);

RoyalTransaction.saveInBackground(Realm, RealmObject...);
RoyalTransaction.saveInBackground(Transaction, OnRoyalUpdatedListener, RealmObject...);

RoyalTransaction.delete(Realm, RealmObject...);
RoyalTransaction.deleteInBackground(Realm, OnRoyalUpdatedListener, RealmObject...);
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