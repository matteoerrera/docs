### Login

``POST /login``

**Group**: [baasbox.account](#list-groups)

Checks username/password and grants the user the right to execute other calls. 
This API returns a session token (**X-BB-SESSION**) that must be provided into 
all authenticated calls.

Parameter | Description
--------- | -----------
**username** | The username for the user. Mandatory
**password** | Password. Mandatory
**appcode** | The appcode of your server instance

This API supports both application/x-www-form-urlencoded and application/json content types.


<div class="snippet-title">
	<p>Sample request to login</p>
</div> 

```shell
curl http://localhost:9000/login \
	-d "username=cesare" \
	-d "password=password" \
	-d "appcode=1234567890"
```

```objective_c
[BAAUser loginWithUsername:@"cesare"
                  password:@"password"
                completion:^(BOOL success, NSError *error) {
                  if (success) {
                    NSLog(@"user is %@", client.currentUser);
                  } else {
                    // show error
                  }
                }];
```

```java
BaasUser user = BaasUser.withUserName("andrea")
                        .setPassword("password");                        
user.login(new BaasHandler<BaasUser>() {
  @Override
  public void handle(BaasResult<BaasUser> result) {
    if(result.isSucces()) {
      Log.d("LOG", "The user is currently logged in: "+result.value());
    } else {
      Log.e("LOG","Show error",result.error());
    }
  }
});
```

```javascript
BaasBox.login("cesare", "password")
	.done(function (user) {
		console.log("Logged in ", user);
	})
	.fail(function (err) {
		console.log("error ", err);
	})
```

<div class="snippet-title">
	<p>Sample response when a user is logged in</p>
</div> 

```json
{
    "result": "ok",
    "data": {
        "user": {
            "name": "cesare",
            "status": "ACTIVE",
            "roles": [{"name": "registered"}]
        },
        "id": "6d5d8300-4339-40a5-8688-d1547fec6e05",
        "visibleByAnonymousUsers": { },
        "visibleByTheUser": { },
        "visibleByFriends": { },
        "visibleByRegisteredUsers": {
            "_social": { }
        },
        "signUpDate": "2015-06-16T16:20:32.032+0200",
        "generated_username": false,
        "X-BB-SESSION": "7f943d99-5872-4f0e-9865-a02386ef882b"
    },
    "http_code": 201
}
```

Here is the explanantion of the server response:

Field | Description
--------- | -----------
**id** | The unique ID associated with the user
**user.name** | The username for the user.
**user.status** | It is always ACTIVE. Users can be suspended by themselves or by an administrator
**visibleByTheUser** | an object whose fields are private and visible only by the user
**visibleByFriends** | an object whose fields are visible by the user and friends (for friendship management)
**visibleByFriends._social** | If the user logged in with Facebook or another supported social network, here are some data that can be useful. For example the FBID.
**visibleByRegisteredUsers** | an object whose fields are visible by the user, friends, any registered user
**signUpDate** | The sign up date and time
**generated_username** | When set to true indicates that the user.name field is a fake string generated by the system. This happens when the user logged in with a social network
**X-BB-SESSION** | The session code that must be used in following calls