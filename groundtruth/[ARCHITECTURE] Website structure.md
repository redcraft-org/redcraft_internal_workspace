# [ARCHITECTURE] Website structure


## API

### UrlShortener V1
```
url: https://redcraft.org/api/v1/urls
description: get list url
methode: GET
```

```
url: https://redcraft.org/api/v1/url
description: set list url
methode: POST
request: {
    token: <str:your_token>,
    shortened: <str:your_token>|Null,
    url: <str:url>
}
```

```
url: https://redcraft.org/r/<str:shortened>
description: redirect to url
methode: GET
```



### Skin V1
    GET: https://redcraft.org/api/v1/skin/template/<str:ref>
    GET: https://redcraft.org/api/v1/head/template/<str:ref>
    GET: https://redcraft.org/api/v1/body/template/<str:ref>

## Pages

### Home
Presentation of the server
- Welcome header
- Basic server presentation
- News
- Deeper presentation of the servers
- Staff list
### Contact
Form to send a message to the staff
### Vote
List of all sites where you can vote for the server
### Stats
- Server statistics
- Player statistics
- Dynmap
### Rules
Minecraft and Discord Server Rules Overview
### Articles
Page regrouping all the articles of the server
--> Reference : https://www.codeply.com/go/u7fEDdUbXg/bootstrap-article-list