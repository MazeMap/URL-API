
# URL Scheme sharing
This document describes the specific `sharemode=url_scheme` functionality of the MazeMap app.
**DRAFT ONLY**

###Short intro
When integrating the MazeMap webapp in native mobile applications, you might want to trigger the native OS's share-dialog, instead of the default share dialog in the MazeMap webapp.

By taking advantage of the native app's ability to listen for specific URL Scheme requests, we have made it possible for web app to communicate with the native app outside the WebView sandbox.

<br>
---
<br>

## 1. Set the `url_scheme` parameter in the url
`use.mazemap.com/?...&sharemode=url_scheme`

<br>
---
<br>
## 2. Inject string variable in window scope
Inject the variable `mazemap_share_urlscheme_string_tmpl` in the `window` scope any time after the map has loaded and set it to the url-scheme you want. This is a template string that you control.

 **JS example:**
```
window.mazemap_share_urlscheme_string_tmpl =
    "customappscheme://sharemap/?mazemapshareprops={{mazemapshareprops}}"
```

**ObjectiveC example (iOS)**
```
[self.webView stringByEvaluatingJavaScriptFromString:@"window.mazemap_share_urlscheme_string_tmpl=\"customappscheme://sharemap/?mazemapshareprops={{mazemapshareprops}}\""];
```

**Java example (Android)**
```
//TODO: Insert here
```
<br>
 The `{{mazemapshareprops}}` works similar to a [Handlebars](http://handlebarsjs.com/) expression and will be automatically replaced by a URLEncoded JSON stringified object, which you can decode and parse as JSON. When you do this, the resulting object will look like:
 ```
    {
      mazemapshareprops:
      {
        longUrl: 'https://use.mazemap.com/?v=1&sharepoitype=...',
        shortUrl: 'http://bit.ly/1337'
      }
    }
 ```
 **NOTE:** the `shortUrl` property might not always be set, so you must check if this is property is a string before using it. Fallback to `longUrl`


<br>
---
<br>
## 3. Listen for URLScheme in native app

**Note** : This is sample code. All error cases are not managed.

##### ObjectiveC (iOS)


```

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType{
    if (SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"6.0")) {
        if (request) {
            NSURL *url = request.URL;

           if ([url.scheme isEqualToString:@"customappscheme"]) {

               //Extract only the query-part of the url
               NSString *query = url.query; //mazemapshareprops=%7B%22longUrl%22%3A%22...

               //Make a dictionary with the parameters found in the query string
               NSMutableDictionary *params = [NSMutableDictionary dictionary];
               for (NSString *param in [query componentsSeparatedByString:@"&"]) {
                   NSArray *elts = [param componentsSeparatedByString:@"="];
                   if([elts count] < 2) continue;
                   [params setObject:[elts lastObject] forKey:[elts firstObject]];
               }

               //Read the url-encoded stringified representation of the JSON object from the dictionary
               NSString *mmSharePropsEncoded = [params objectForKey:@"mazemapshareprops"];

               //Decode the string to get the proper JSON-stringified version
               NSString *mmSharePropsDecoded = (__bridge_transfer NSString *)CFURLCreateStringByReplacingPercentEscapesUsingEncoding(NULL, (CFStringRef)mmSharePropsEncoded, CFSTR(""), kCFStringEncodingUTF8);

               NSError *error;
               //Parse the string and make a proper JSON object
               NSData *jsonData  = [mmSharePropsDecoded dataUsingEncoding:NSUTF8StringEncoding];
               NSMutableDictionary *jsonDict = [NSJSONSerialization JSONObjectWithData:jsonData options:NSJSONReadingMutableContainers error:&error];

               //Now we can read the actual properties shared from the mazemap webapp
               NSString *shareUrl = @"";
               if ([jsonDict objectForKey:@"shortUrl"]) {
                   shareUrl = [jsonDict valueForKey:@"shortUrl"];
               } else if ([jsonDict objectForKey:@"longUrl"]) { //Fallback to longUrl
                   shareUrl = [jsonDict valueForKey:@"longUrl"];
               }

                @try {
                    NSURL *url = [NSURL URLWithString:[shareUrl stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding]];

                    //This is where you decide what to do with the shared url
                   [self showNativeShareMenu:url];

                }
                @catch (NSException *exception) {
                    NSLog(@"share url exception %@", [exception description]);
                }
                @finally {

                }
                return NO;
            }
        }
    }
    return YES;
}

```
**Java example (Android)**
```
//TODO: Insert here
```

<br>
---
<br>
## 4. Test

Now you should be able to see that nothing happens in the webapp when clicking the "share" button, and you should be able to perform an alternative action in the native app.

<br><br>
