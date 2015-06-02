#SnapHTTP

An incredibly simple HTTP client library for Swift.

## Features

- Fantastically basic closure API with chaining.
- Support for JSON, NSData, [UInt8], String body content.
- QueryString and Form encoding.
- Builtin JSON serialization
- Request image data directly into UIImage.
- Supports GET, POST, PUT, HEAD, DELETE, PATCH, OPTIONS.

##Install (iOS and OS X)

###CocoaPods

You can use [CocoaPods](http://cocoapods.org/?q=SnapHTTP) to install the `SnapHTTP` framework.

Add the following lines to your `Podfile`.

```ruby
use_frameworks!
pod 'SnapHTTP'
```

The `import SnapHTTP` directive is required in order to access SnapHTTP features.

###Drop-in

There is no need for `import SnapHTTP` when manually installing.

##Examples

### GET

A basic GET request.

```swift
http.get("http://www.google.com") { resp in
    println("response: \(resp.string)")
}
```

Adding parameters to the request.

```swift
http.get("http://www.google.com").params(["q": "swift lang"]) { resp in
    println("response: \(resp.string)")
}
```

JSON response.

```swift
http.get("https://ajax.googleapis.com/ajax/services/search/web").params(["q": "Emily Dickinson", "v": "1.0"]) { resp in
    println("JSON: \(resp.json)")
}
```

Binary data response. NSData or [UInt8].

```swift
http.get("https://www.google.com/images/logo.png") { resp in
    println("[UInt8]: \(resp.data.length) bytes")
    println("NSData: \(count(resp.bytes)) bytes")
}
```

UIImage response.

```swift
http.get("https://www.google.com/images/logo.png") { resp in
    println("UIImage: \(resp.image.size)")
}
```

### POST

Using the `params` method will serialize the input as form data.

```swift
http.post("https://api.twitter.com/1.1/statuses/update.json").params(["status": "Or else a peacock’s purple train"]) { resp in
    println("response: \(resp.string)")
}
```

Posting JSON.

```swift
http.post("https://api.twitter.com/1.1/statuses/update.json").body(["status": "Or else a peacock’s purple train"]) { resp in
    println("response: \(resp.string)")
}
```

Posting a string.

```swift
http.post("http://domain.com").body("plain text sent to server") { resp in
    println("response: \(resp.string)")
}
```

Posting binary. This can be a [UInt8], NSData, or NSInputStream

```swift
var data : [UInt8] = [/* some good data */]
http.post("http://domain.com").body(data) { resp in
    println("response: \(resp.string)")
}
```

### Custom Headers

```swift
var imageData = NSData() // pretend we have some jpeg data 
http.post("http://domain.com").header(["Content-Type", "image/jpeg"]).body(imageData) { resp in
    println("response: \(resp.string)")
}
```

### Cancelling

```swift
var req = http.get("http://google.com") { resp in
    println("response: \(resp.string)")
}
req.cancel()
```

### Error Handling

```swift
var req = http.get("badscheme://google.com") { resp in
    if resp.error != nil {
        println("Connection error: \(resp.error!)")
        return
    }
    println("response: \(resp.string)")
}
```

## Contact
Josh Baker [@tidwall](http://twitter.com/tidwall)

## License

The SwiftWebSocket source code is available under the MIT License.
