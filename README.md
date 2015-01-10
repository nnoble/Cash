# Cash
A thin HTTP Request library focused on local caching, and written in Swift

For iOS 8.1, NSURLSession and NSURLCache don't seem to play nice together... so I created <b>Cash</b>. <b>Cash</b> uses NSURLConnection instead of NSURLSession.

For a full list of functionality, check out the <a href="https://github.com/nnoble/Cash/wiki">wiki</a>

## How does it work?

<b>Cash</b> intercepts HTTP responses, and changes the "Cache-Control" header to allow NSURLCache to cache your responses for however long you want.

## When should I use Cash?

1. If you want a simple/easy way to cache your web requests and control their expiration times
2. If you don't need any of the features that NSURLSession gives you (background tasks, pause & continue tasks, etc.)

## Requirements

- iOS 7.0+ / Mac OS X 10.9+
- Xcode 6.1

## Installation

- For Projects, just drag Cash.swift to the project tree.
- For Workspaces, include the whole Cash.xcodeproj

## Usage

### Setting up NSURLCache
Add a shared NSURLCache to your AppDelegate
```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
  let urlCache = NSURLCache(memoryCapacity: 20*1024*1024, diskCapacity: 20*1024*1024, diskPath: nil)
  NSURLCache.setSharedURLCache(urlCache)
  return true
}
```

### Making a GET Request

```swift
import Cash

//This request will be cached for 60 seconds
Cash.shared.get("http://httpbin.org/get", expiration: 60) { (data, error) -> Void in
  println(data)
}
```

### Making a POST Request

```swift
import Cash

let params = ["foo" : "bar"]
//This request will be cached for 1 hour
Cash.shared.post("http://httpbin.org/post", body: params, expiration: 60*60) { (data, error) -> Void in
  println(data)
}
```

### Making Any Other Request

```swift
import Cash

let params = ["foo" : "bar"]
let body = NSJSONSerialization.dataWithJSONObject(params, options: nil, error: nil)
Cash.shared.request(HTTPMethod.PUT, url: "http://httpbin.org/put", body: body, expiration: 60) { (data, error) -> Void in
  println(data)
}
```

### Pass in a NSMutableURLRequest

```swift
import Cash

let request = NSMutableURLRequest(URL: NSURL(string: "http://httpbin.org/get")!)
//Set the expiration to 'nil' to use the Cache-Control value returned from the server.
Cash.shared.sendRequest(request, expiration: nil) { (data, error) -> Void in
  println(data)
}
```
