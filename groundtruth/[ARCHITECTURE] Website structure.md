# [ARCHITECTURE] Website structure

[![hackmd-github-sync-badge](https://hackmd.io/0dh9taMIQeidOYiwYMYEvg/badge)](https://hackmd.io/0dh9taMIQeidOYiwYMYEvg)



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
params post:
  - token: str, token to provider
  - shortened: str|null, choise shortened if null, hash is generate to shortene, 
  - url: str: url to redirect
```

```
url: https://redcraft.org/r/<str:shortened>
description: redirect to url
methode: GET
```



### Skin V1
```
url: https://redcraft.org/api/v1/skin/template/<str:ref>
params url:
  - ref: uuid or username to player

description: get template to player
methode: GET
params get:
  - size: int, size to img skin
```

```
url: https://redcraft.org/api/v1/skin/head/<str:ref>
params url: {
    ref: uuid or username to player
}
description: get head to player
methode: GET
params:
  - size: int, size to img skin
  - outer: bool, if print outer in skin
```

```
url: https://redcraft.org/api/v1/skin/body/<str:ref>
params url: {
    ref: uuid or username to player
}
description: get full skin face to player
methode get: GET
params:
  - size: int, size width to img skin
  - outer: bool, if print outer in skin
```

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
### Rules & References
Minecraft and Discord Server Rules Overview
References used for creating the website (& server ?)
- [Fontawesome license](https://fontawesome.com/license)

### Articles
Page regrouping all the articles of the server
--> Reference : https://www.codeply.com/go/u7fEDdUbXg/bootstrap-article-list