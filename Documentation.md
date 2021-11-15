# **`Auth`**

## **`LogIn`**

### Request:

`AUTH /POST /login`

### Params:

Body

- usernameOrEmail
- password

### Example:

```js
axios
	.post(
		`url/api/auth/login`,
		{
			usernameOrEmail: "test@gmail.com",
			password: "1234Q@werty",
		},
		{}
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
	"user": {
		"_id": "618a8e39bbd3581aa49fc8be",
		"email": "test@gmail.com",
		"username": "test"
	},
	"token": "eyJ0eXAiOi..."
}
```

---

## **`SignUp`**

### Request:

`AUTH /POST /register`

### Params:

Body

- username
- fullName
- email
- password

### Example:

```js
axios
	.post(`url/api/auth/register`, {
		username: "test",
		fullName: "first test",
		email: "test@gmail.com",
		password: "12345@Qwerty",
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 201 Created
{
	"user": {
		"email": "test@gmail.com",
		"username": "test"
	},
	"token": "eyJ0eXAi..."
}
```

---

## **`Change password`**

### Request:

`AUTH /PUT /password`

### Params:

**Require auth**

Headers

- authorization

Body

- oldPassword
- newPassword

### Example:

```js
axios
	.put(
		`url/api/auth/register`,
		{
			newPassword: "@Hello1234",
			oldPassword: "1234Q@werty",
		},
		{ authorization: yourToken }
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{}
```

# **`User`**

## **`Retrieve user`**

### Request:

`USER /GET /:username`

### Params:

**Optional auth**

### Example:

```js
axios.get(`url/api/user/:username`).then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
	"user": {
		"_id": "618a8e39bbd3581aa49fc8be",
		"username": "test",
		"fullName": "first test",
		"bookmarks": []
	},
	"followers": 0,
	"following": 0,
	"isFollowing": false
}
```

## **`Retrieve posts`**

### Request:

`USER /GET /:username/posts/:offset`

### Example:

```js
axios.get(`url/api/user/test1/posts/0`).then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
	{
	    "_id": "618bb82094f97748f4cf807f",
	    "image": "https://res.cloudinary.com/...",
	    "caption": "hello world its first test #post #firstPost",
	    "date": "2021-11-10T12:16:32.743Z",
	    "user": [
	      {
	        "username": "test1"
	      }
	    ],
	    "comments": 0,
	    "postVotes": 0
	},
]
```

## **`Retrieve following`**

### Request:

`USER /GET /:userId/:offset/following`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.get("url/api/user/618b7ae45e398e57f4726d20/0/following", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
	{
		"_id": "618b86ea5e398e57f4726d28",
		"username": "test3",
		"fullName": "ttest3",
		"isFollowing": true
	}
]
```

## **`Retrieve followers`**

### Request:

`USER /GET /:userId/:offset/followers`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.get("url/api/user/618b86e05e398e57f4726d24/0/followers", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
	{
		"_id": "618b7ae45e398e57f4726d20",
		"username": "test1",
		"fullName": "first test1",
		"isFollowing": false
	}
]
```

## **`Search users`**

### Request:

`USER /GET /:username/:offset/search`

### Example:

```js
axios.get(`url/api/user/api/user/test4/0/search`).then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
	{
		"_id": "618b86f25e398e57f4726d2c",
		"username": "test4",
		"fullName": "ttest4"
	}
]
```

## **`Retrieve suggested users`**

### Request:

`USER /GET /suggested/:max`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios.get(`url/api/user/api/user/suggested/2`, { authorization: yourToken }).then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
	{
		"_id": "618b86ea5e398e57f4726d28",
		"username": "test3",
		"fullName": "ttest3",
		"email": "test3@gmail.com",
		"posts": []
	},
	{
		"_id": "618b86e05e398e57f4726d24",
		"username": "test2",
		"fullName": "ttest2",
		"email": "test2@gmail.com",
		"posts": []
	}
]
```

## **`Confirm user`**

### Request:

`USER /PUT /confirm`

### Params:

**Require auth**

Headers

- authorization

Body

- token

### Example:

```js
axios
	.put(
		`url/api/user/confirm`,
		{
			token: "ff8e5891bd4f...",
		},
		{ authorization: yourToken }
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{

}
```

## **`Change avatar`**

### Request:

`USER /PUT /avatar`

### Params:

**Require auth**

Headers

- authorization

Body

- file

```js
const formData = new FormData();
formData.append("image", event.target.files[0]);
axios
	.put(
		`url/api/user/avatar`,
		{
			formData,
		},
		{ authorization: yourToken }
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
	"avatar": "Cloudinary link"
}
```

## **`Update profile`**

### Request:

`USER /PUT /`

### Params:

**Require auth**

Headers

- authorization

Body

- fullName
- username
- website
- bio
- email

### Example:

```js
axios
	.put(
		`url/api/user/`,
		{
			website: "test.com",
			email: "newtest.com",
		},
		{ authorization: yourToken }
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
	"website": "test.com",
	"email": "newtest.com"
}
```

## **`Remove avatar`**

### Request:

`USER /DEL /avatar`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios.delete(`url/api/user/avatar`, { authorization: yourToken }).then((response) => console.log(response));
```

### Success Response:

```json
Status: 204 No Content
{

}
```

## **`Add post to bookmark`**

### Request:

`USER /POST /:postId/bookmark`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.post(
		"url/api/user/618bb71682719166e4afe5ef/bookmark",
		{},
		{
			headers: {
				authorization: yourToken,
			},
		}
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
  	"success": true,
  	"operation": "add"
}
OR
{
  	"success": true,
  	"operation": "remove"
}

```

## **`Follow the user`**

### Request:

`USER /POST /:userId/follow`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.post(`url/api/618b86e05e398e57f4726d24/follow`, {}, { authorization: yourToken })
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 204 OK
{
  	"success": true,
  	"operation": "follow"
}
OR
{
	"success": true,
  	"operation": "unfollow"
}
```

# **`Post`**

## **`Create post`**

### Request:

`POST /POST /`

### Params:

**Require auth**\
**Have a limit of 5 times every 15 minutes**\
**Limit upload file size is 10 mb**

Headers

- authorization

Body

- caption
- image
- filter?

```js
const formData = new FormData();
formData.append("image", event.target.files[0]);
formData.set("caption", "hello world its first test #post #firstPost");
axios
	.post("url/api/post/", formData, {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 201 CREATED
{
	"author": {"username": "test1"},
	"caption": "hello world its first test post",
	"comments": [],
	"date": "2021-11-10",
	"filter": "",
	"hashtags": ["post","firstPost"],
	"image": "https://res.cloudinary.com/...",
	"postVotes": [],
	"__v": 0,
	"_id": "618bb62482719166e4afe5ed",
}
```

## **`Like`**

### Request:

`POST /POST /:postId/vote`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.post(
		"url/api/post/618bb71682719166e4afe5ef/vote",
		{},
		{
			headers: {
				authorization: yourToken,
			},
		}
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
	"success": true
}
```

## **`Get suggested posts`**

### Request:

`POST /GET /suggested/:offset`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.get(
		"url/api/post/suggested/0",
		{},
		{
			headers: {
				authorization: yourToken,
			},
		}
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
	{
    	"_id": "618bb82094f97748f4cf807f",
    	"hashtags": [
    	  "post",
    	  "firstpost"
    	],
    	"image": "https://res.cloudinary.com/...",
    	"filter": "",
    	"caption": "hello world its first test #post #firstPost",
    	"author": {
    	  "_id": "618b7ae45e398e57f4726d20",
    	  "username": "test1",
    	  "fullName": "first test1",
    	  "email": "test@gmail.com",
    	  "__v": 0
    	},
    	"date": "2021-11-10",
    	"__v": 0,
    	"comments": 0,
    	"postVotes": 0
  }
]
```

## **`Filter`**

### Request:

`POST /GET /filters`

### Example:

```js
axios.get("url/api/post/filters").then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
  "filters": [
    {
      "name": "Clarendon",
      "filter": "saturate(2)"
    },
    {
      "name": "Gingham",
      "filter": "contrast(0.7) saturate(1.5)"
    },
    {
      "name": "Moon",
      "filter": "grayscale(1)"
    },
    {
      "name": "Lark",
      "filter": "saturate(1.6) hue-rotate(15deg)"
    },
    {
      "name": "Reyes",
      "filter": "contrast(0.7)"
    },
    {
      "name": "Juno",
      "filter": "hue-rotate(-20deg)"
    }
  ]
}
```

## **`Get post`**

### Request:

`POST /GET /:postId`

### Example:

```js
axios.get("url/api/post/618bb71682719166e4afe5ef").then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
	"_id": "618bb71682719166e4afe5ef",
	"hashtags": [
		"post",
		"firstpost"
	],
	"image": "https://res.cloudinary.com/...",
	"filter": "",
	"caption": "hello world its first test #post #firstPost",
	"author": {
		"_id": "618b7ae45e398e57f4726d20",
		"confirmed": false,
		"username": "test1",
		"fullName": "first test1",
		"bookmarks": [],
		"__v": 0
	},
	"date": "2021-11-10T12:12:06.178Z",
	"__v": 0,
	"postVotes": []
}
```

## **`Get posts in the feed`**

### Request:

`POST /GET /feed/:offset`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.get("url/api/post/feed/0", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
    "_id": "618bb82094f97748f4cf807f",
    "hashtags": [
      "post",
      "firstpost"
    ],
    "image": "https://res.cloudinary.com/...",
    "filter": "",
    "caption": "hello world its first test #post #firstPost",
    "author": {
      "_id": "618b7ae45e398e57f4726d20",
      "username": "test1",
      "fullName": "first test1",
      "__v": 0
    },
    "date": "2021-11-10T12:16:32.743Z",
    "__v": 0,
    "postVotes": [],
    "commentData": {
      "comments": []
    }
},
```

## **`Get a hashtag posts`**

### Request:

`POST /GET /hashtag/:hashtag/:offset`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.get("url/api/post/hashtag/post/0", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
  	"posts": [
    	{
			"_id": "618bb71682719166e4afe5ef",
			"hashtags": [
				"post",
				"firstpost"
			],
			"image": "https://res.cloudinary.com/...",
			"filter": "",
			"caption": "hello world its first test #post #firstPost",
			"author": {
				"_id": "618b7ae45e398e57f4726d20",
				"username": "test1",
				"fullName": "first test1",
				"email": "test@gmail.com",
				"__v": 0
			},
			"date": "2021-11-10T12:12:06.178Z",
			"__v": 0,
			"comments": 0,
			"postVotes": 0
    	},
	]
}
```

## **`Delete post`**

### Request:

`POST /DELETE /:postId`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.delete("url/api/post/618bb71682719166e4afe5ef", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 204 No Content
{

}
```

# **`Comment`**

## **`Create comment`**

### Request:

`COMMENT /POST /:postId`

### Params:

**Require auth**

Headers

- authorization

Body

- message

### Example:

```js
axios
	.post(
		"url/api/comment/618bb62482719166e4afe5ed",
		{ message: "hello yur post are awesome" },
		{
			headers: {
				authorization: yourToken,
			},
		}
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 201 Created
{
	"_id": "618be3af94f97748f4cf8089",
	"message": "hello yur post are awesome",
	"author": {
		"username": "test1"
	},
	"post": "618bb62482719166e4afe5ed",
	"date": "2021-11-10",
	"__v": 0,
	"commentVotes": []
}
```

## **`Like comment`**

### Request:

`COMMENT /POST /:postId/vote`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.post(
		"url/api/comment/618be3af94f97748f4cf8089/vote",
		{},
		{
			headers: {
				authorization: yourToken,
			},
		}
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
  "success": true
}
```

## **`Create comment reply`**

### Request:

`COMMENT /POST /:parentCommentId/reply`

### Params:

**Require auth**

Headers

- authorization

Body

- message

### Example:

```js
axios
	.post(
		"url/api/comment/618be3af94f97748f4cf8089/reply",
		{},
		{
			headers: {
				authorization: yourToken,
			},
		}
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 201 Created
{
	"_id": "618be60894f97748f4cf8092",
	"parentComment": "618be3af94f97748f4cf8089",
	"message": "hello you are right",
	"author": {
		"username": "test1"
	},
	"date": "2021-11-10T15:32:24.522Z",
	"__v": 0,
	"commentReplyVotes": []
}
```

## **`Like comment reply`**

### Request:

`COMMENT /POST /:commentReplyId/replyVote`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.post(
		"url/api/comment/618be60894f97748f4cf8092",
		{},
		{
			headers: {
				authorization: yourToken,
			},
		}
	)
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
  "success": true
}
```

## **`Get comments`**

### Request:

`COMMENT /GET /:postId/:offset/:exclude`

### Example:

```js
axios.get("url/api/comment/618bb62482719166e4afe5ed/0/0").then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
{
	"comments": [
		{
		"_id": "618be3af94f97748f4cf8089",
		"message": "hello yur post are awesome",
		"author": {
			"_id": "618b7ae45e398e57f4726d20",
			"confirmed": false,
			"username": "test1",
			"fullName": "first test1",
			"__v": 0
		},
		"post": "618bb62482719166e4afe5ed",
		"date": "2021-11-10T15:22:23.078Z",
		"__v": 0,
		"commentReplies": 1,
		"commentVotes": [
			{
			"_id": "618be53c94f97748f4cf8091",
			"author": "618b7ae45e398e57f4726d20"
			}
		]
		}
  ],
  "commentCount": 1
}
```

## **`Get replies to comments`**

### Request:

`COMMENT /GET /:parentCommentId/:offset/replies/`

### Example:

```js
axios.get("url/api/comment/618be3af94f97748f4cf8089/0/replies").then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
  {
    "_id": "618be60894f97748f4cf8092",
    "parentComment": "618be3af94f97748f4cf8089",
    "message": "hello you are right",
    "author": {
      "_id": "618b7ae45e398e57f4726d20",
      "confirmed": false,
      "username": "test1",
      "fullName": "first test1",
      "__v": 0
    },
    "date": "2021-11-10T15:32:24.522Z",
    "__v": 0,
    "commentReplyVotes": [
      {
        "_id": "618be68e94f97748f4cf8096",
        "author": "618b7ae45e398e57f4726d20"
      }
    ]
  }
]
```

## **`Delete comment`**

### Request:

`COMMENT /DELETE /:commentId`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.delete("url/api/comment/618be3af94f97748f4cf8089", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 204 No Content
{

}
```

## **`Delete comment reply`**

### Request:

`COMMENT /DELETE /:commentReplyId/reply`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.delete("url/api/comment/618be3af94f97748f4cf8089/reply", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 204 No Content
{

}
```

# Notification

## **`Receive notifications`**

### Request:

`NOTIFICATION /GET /`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.get("url/api/notification/", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
	{
		"_id": "618ba9945e398e57f4726d37",
		"read": false,
		"notificationType": "follow",
		"sender": {
		"_id": "618b7ae45e398e57f4726d20",
		"username": "test1"
		},
		"receiver": {
		"_id": "618b86ea5e398e57f4726d28"
		},
		"date": "2021-11-10T11:14:28.934Z",
		"isFollowing": false
	}
]
```

## **`Read notifications`**

### Request:

`NOTIFICATION /PUT /`

### Params:

**Require auth**

Headers

- authorization

### Example:

```js
axios
	.get("url/api/notification/", {
		headers: {
			authorization: yourToken,
		},
	})
	.then((response) => console.log(response));
```

### Success Response:

```json
Status: 200 OK
[
	{
		"_id": "618ba9945e398e57f4726d37",
		"read": true,
		"notificationType": "follow",
		"sender": {
		"_id": "618b7ae45e398e57f4726d20",
		"username": "test1"
		},
		"receiver": {
		"_id": "618b86ea5e398e57f4726d28"
		},
		"date": "2021-11-10T11:14:28.934Z",
		"isFollowing": false
	}
]
```
