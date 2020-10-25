## Welcome to core documents

### Extensions

Custom created extensions

### Save data object classes in preferences

In your shared preference, when you need to save an object class just call `saveToJson(key, value)`. Parameter `key` is a value of string use to assign as an identifier and `value` is any instance of an object classes.

```kotlin
  pref.saveToJson(key, value)
```


In your shared preference, when you need to retrieve an object class just call `jsonToOject<T>(key)`. Parameter `key` is a value of string use to assign as an identifier and `T` is to specify which data class you have previously use to save in shared preference otherwise it will return `null`

```kotlin
  pref.jsonToObject<T>(key)
```

For example:

```kotlin
  data class User(val id: String)
  
  val prefs = // SharedPreference
  const val KEY_USER = "key_user"
  
  val myUser = User("my-id")
  
  prefs.saveToJson(KEY_USER, myUser)
  // the object will be added in shared preference .xml and formatted as json string
  // { "id" : "my-user" }
  
  val user = prefs.jsonToObject<User>(KEY_USER)
  // the object will be retrieve frin shared preference .xml and formatted as specified data class (in this case `User`)
  // { "id" : "my-user" }
```
