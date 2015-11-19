# node-mattermost

A node module for sending and receiving messages with Mattermost via webhooks.

[Mattermost](http://www.mattermost.org/) is a messaging platform that is easy to integrate with.
This module should be useful for creating various integrations with Mattermost, such as
chat bots!

## Install Mattermost

node-mattermost is available via npm:

```
npm install node-mattermost
```


Get your hook_url from the Mattermost Incoming Webhooks Integration page.

```
var Mattermost = require('node-mattermost');
var mattermost = new Mattermost(hook_url,options);
```

If your system requires that requests be made through
an HTTP or HTTPS proxy, you can either set an environment
variables https_proxy and http_proxy,
or pass in the optional third option:

```
var mattermost = new Mattermost(hook_url,{proxy: http_proxy});
```

To send a message, call mattermost.send:

```
mattermost.send({
	text: 'Howdy!',
	channel: '#foo',
	username: 'Bot'
});
```

You can also specify an emoji icon, a url to a custom icon, attachments,
and any of the other options listed [here](https://github.com/mattermost/platform/blob/master/doc/integrations/webhooks/Incoming-Webhooks.md).


```
mattermost.send({
	text: 'Howdy!',
	channel: '#foo',
	username: 'Bot',
	icon_emoji: 'taco',
	attachments: attachment_array,
	unfurl_links: true,
	link_names: 1
});
```



To respond to an outgoing webhook from mattermost, pass the information from the webhook into mattermost.respond,
along with a callback function responsible for returning a response.

From inside an Express.js route, this is as easy as passing in req.body:

```
app.post('/yesman',function(req,res) {

	var reply = mattermost.respond(req.body,function(hook) {

		return {
			text: 'Good point, ' + hook.user_name,
			username: 'Bot'
		};

	});

	res.json(reply);

});

```

### Notes

Based on [node-slack](https://github.com/xoxco/node-slack)