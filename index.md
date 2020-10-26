# Welcome to Core CMS and Extensions guide documents

## CMS services and parser

The CMS parser is just a simple serialization of json objects. It will convert json string to a data class objects. But the unique part of this is it will dynamically build an object based on Schema.

The currently supported schemas are:  

|Data object class|Schema enumeration           |Schema value|
|-----------------|-----------------------------|------------|
|`MobileContent`    |`MOBILE`                       |`https://cebu.onecms.slot.mobile`|
|`PageContent`      |`PAGE`                         |`https://cebu.onecms.content.page`|
|`DynamicPageContent`|`DYNAMIC_PAGE`                 |`https://cebu.onecms.content.dynamicpage`|
|`SectionBlockContent`|`SECTION_BLOCK`                |`https://cebu.onecms.content.sectionblock`|
|`ImageAndTextContent`|`IMAGE_AND_TEXT`               |`https://cebu.onecms.content.imageandtext`|
|`MessageContent`   |`MESSAGE`                      |`https://cebu.onecms.content.message`|
|`ImageContent`    |`IMAGE`                        |`https://cebu.onecms.content.image`|
|`LinkContent`      |`LINK`                         |`https://cebu.onecms.content.link`|
|`BannerContent`    |`BANNER`                       |`https://cebu.onecms.content.banner`|
|`ImageCarouselContent`|`IMAGE_CAROUSEL`               |`https://cebu.onecms.content.imagecarousel`|
|`BannerCarouselContent`|`BANNER_CAROUSEL`              |`https://cebu.onecms.content.bannercarousel`|
|`PromotionalImage` |`PROMOTIONAL_IMAGE`            |`https://cebu.onecms.content.promotionalimage`|
|`TextContent`      |`TEXT_CONTENT`                 |`https://cebu.onecms.content.textcontent`|
|`TravelReminderContent`|`TRAVEL_REMINDER`              |`https://cebu.onecms.content.travelreminder`|
|`HeaderContent`    |`HEADER`                       |`https://cebu.onecms.content.header`|
|`SocialMediaLinksContent`|`SOCIAL_MEDIA_LINKS`           |`https://cebu.onecms.content.socialmedialinks`|
|`HeroBannerContent`|`HERO_BANNER`                  |`https://cebu.onecms.content.herobanner`|
|`NavigationMenuContent`|`NAVIGATION_MENU`              |`https://cebu.onecms.content.navigationmenu`|
|`BannerNavigationLinksContent`|`BANNER_NAVIGATION_LINKS`      |`https://cebu.onecms.content.bannernavigationlinks`|
|`BannerNavigationLinksAndTextContent`|`BANNER_NAVIGATION_LINKS_AND_TEXT`|`https://cebu.onecms.content.bannernavigationlinksandtextcontent`|
|`ImageGridContent` |`IMAGE_GRID`                   |`https://cebu.onecms.content.imagegrid`|
|`ImageGridCarouselContent`|`IMAGE_GRID_CAROUSEL`          |`https://cebu.onecms.content.imagegridcarousel`|
|`FlightMapContent` |`FLIGHT_MAP`                   |`https://cebu.onecms.content.flightmap`|
|`RegionContent`    |`REGION`                       |`https://cebu.onecms.content.region`|
|`ArticleListContent`|`ARTICLE_LIST`                 |`https://cebu.onecms.content.articlelist`|
|`ArticleContent`   |`ARTICLE`                      |`https://cebu.onecms.content.article`|
|`ImageLinkContent` |`IMAGE_LINK `                  |`http://bigcontent.io/cms/schema/v1/core#/definitions/image-link`|
|                 |`LOCALIZED_VALUE`              |`http://bigcontent.io/cms/schema/v1/core#/definitions/localized-value`|

Example flow:

We have this example meta json content, the `_meta` and `schema` value will use as reference and to create an object dynamically.

```json
{
  "content": {
      "_meta": {
          "name": "Prepaid Baggage Mobile App Slot",
          "schema": "https://cebu.onecms.slot.mobile",
          "deliveryKey": "addons/prepaid-baggage",
          "deliveryId": "dbc74e0f-3fb7-4c8e-a3db-1fbc491978e8"
      },
      "content": []
  }
}
```

In the json content above. `MobileContent` data class is use as an object

```kotlin
@Serializable
data class MobileContent(
    @SerialName("_meta")
    override val meta: Meta,
    val content: List<BaseContent>
) : BaseContent()
```

Assume the field `content` is a list of meta content. The parser will use data class `BaseContent` as a source of magic. It will search for a registered schema and build it dynamically. 

# How to create and register a new schema

TODO

# Extensions and Utilities

Below are the currently supported helpers:

- [ ] Build a link from `ImageLinkContent` and `ImageContent`
- [ ] Get specific image (`ImageCarouselContent`) by `externalIdentifier`
- [ ] Retrieve all page contents from `MobileContent`
- [ ] Get `SectionBlockContent` by meta name
- [ ] Get specified content from `PageContent` by meta name
- [ ] Get locale value by locale (defaule: `en-PH`)
- [X] Saving and Retrieving json to objects (vice versa)

### Saving and Retrieving data object classes in shared preferences

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
