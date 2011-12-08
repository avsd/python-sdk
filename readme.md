Facebook Python SDK
====

This client library is designed to support the
[Facebook Graph API](http://developers.facebook.com/docs/api) and the official
[Facebook JavaScript SDK](http://github.com/facebook/connect-js), which is
the canonical way to implement Facebook authentication. You can read more
about the Graph API at [http://developers.facebook.com/docs/api](http://developers.facebook.com/docs/api).

Basic usage:

    graph = facebook.GraphAPI(oauth_access_token)
    profile = graph.get_object("me")
    friends = graph.get_connections("me", "friends")
    graph.put_object("me", "feed", message="I am writing on my wall!")

In some cases Graph API redirects to different URL, which contains raw data
rather than JSON. For example, for profile picture. The GraphAPI object
in this case returns a dictionary with following keys:

 * **data** - raw data, loaded from the redirecting URL;
 * **url** - actual URL, where Graph API call was redirected;
 * **code** - final HTTP status code;
 * **info** - meta-information, associated with the URL (see http://docs.python.org/library/urllib.html#urllib.urlopen for more details)

Usage with profile pictures:

    graph = facebook.GraphAPI()
    response = get_object('facebook/picture', type='large')
    with open(response['url'].rpartition('/')[2], 'wb') as file:
        file.write(response['data'])

If you are using the module within a web application with the
[JavaScript SDK](http://github.com/facebook/connect-js), you can also use the
module to use Facebook for login, parsing the cookie set by the JavaScript SDK
for logged in users. For example, in Google AppEngine, you could get the
profile of the logged in user with:

    user = facebook.get_user_from_cookie(self.request.cookies, key, secret)
    if user:
        graph = facebook.GraphAPI(user["oauth_access_token"])
        profile = graph.get_object("me")
        friends = graph.get_connections("me", "friends")

You can see a full AppEngine example application in examples/appengine.

Reporting Issues
--------

If you have bugs or other issues, file them [here][issues].

[issues]: http://bugs.developers.facebook.net/enter_bug.cgi?product=SDKs

