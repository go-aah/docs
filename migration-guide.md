## Migration Guide

aah getting improved every release, with `v0.11.0` migration took next step 😊

#### Update aah framework

```bash
# Since v0.10.0 release
aah update

# OR
go get -u aahframework.org/tools.v0/aah
```

#### Migrate application 

```bash
# Since v0.11.0 release
aah migrate code --importpath github.com/user/appname

# OR
aah m c -i github.com/user/appname
```


#### Run application 

```bash
aah run --importpath github.com/user/appname
```