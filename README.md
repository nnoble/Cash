# Cash
A Swift HTTP Request library focused on forcing local cache (uses NSURLCache and NSURLConnection)

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
