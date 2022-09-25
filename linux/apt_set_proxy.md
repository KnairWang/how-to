setup proxy for apt (aptitude):

ref: [StackOverflow](https://stackoverflow.com/questions/41502868/how-to-set-proxy-for-terminal-in-ubuntu)
the accepted answer:
> Edit the /etc/apt/apt.conf file and add the following:
> ```
> Acquire::http::proxy "http://ProxyHOST:ProxyPORT";
> Acquire::https::proxy "https://ProxyHOST:ProxyPORT";
> ```

Basically, I think the format is:
```
{index} {proxy URI}
```
In this case, the `{index}` can be:
```
Acquire::http::proxy
Acquire::https::proxy
```
And the `{proxy URI}` is just a normal URI. The scheme of the URI should match your proxy but not match the index. It means you can use exactly same URI for both two indexes.
For example:
```
"http://127.0.0.1:1080"
```
or:
```
"http://user@127.0.0.1:1080"
```

And for the file, it does not have to be the `/etc/apt/apt.conf`, there could also be a directory, named `/etc/apt/apt.conf.d`, you can also insert these lines into one file under that folder or create your own.