# go-multiaddr

[multiaddr](https://github.com/jbenet/multiaddr) implementation in Go.

## Example

### Simple

```go
import "github.com/jbenet/go-multiaddr"

m := multiaddr.NewString("/ip4/127.0.0.1/udp/1234")
// <Multiaddr /ip4/127.0.0.1/udp/1234>
m.buffer
// <Buffer >
m.String()
// /ip4/127.0.0.1/udp/1234

// construct with Buffer
m = multiaddr.Multiaddr{ Bytes: m.Bytes }
// <Multiaddr /ip4/127.0.0.1/udp/1234>
```

### Protocols

```go
// get the multiaddr protocol codes
m.ProtoCodes()
// []int{4, 6}

// get the multiaddr protocol string codes
m.ProtoNames()
// []string{"ip4", "tcp"}

// get the multiaddr protocol description objects
addr.Protos()
// []Protocol{
//   Protocol{ Code: 4, Name: 'ip4', Size: 32},
//   Protocol{ Code: 17, Name: 'udp', Size: 16},
// }
```

### Other formats

```go
// handles the stupid url version too
m = multiaddr.NewUrl("udp4://127.0.0.1:1234")
// <Multiaddr /ip4/127.0.0.1/udp/1234>
m.Url(buf)
// udp4://127.0.0.1:1234
```

### En/decapsulate

```go
m.Encapsulate(m.NewString("/sctp/5678"))
// <Multiaddr /ip4/127.0.0.1/udp/1234/sctp/5678>
m.Decapsulate(m.NewString("/udp")) // up to + inc last occurrence of subaddr
// <Multiaddr /ip4/127.0.0.1>
```

### Tunneling

Multiaddr allows expressing tunnels very nicely.

```js
printer := multiaddr.NewString("/ip4/192.168.0.13/tcp/80")
proxy := multiaddr.NewString("/ip4/10.20.30.40/tcp/443")
printerOverProxy := proxy.Encapsulate(printer)
// <Multiaddr /ip4/10.20.30.40/tcp/443/ip4/192.168.0.13/tcp/80>

proxyAgain := printerOverProxy.Decapsulate(multiaddr.NewString("/ip4"))
// <Multiaddr /ip4/10.20.30.40/tcp/443>
```
