# beets

[beets.io](http://beets.io/)

```console
$ docker run -it --rm \
	-u "$(id -u):$(id -g)" \
	-v "$PWD:/cwd" \
	-w /cwd \
	-v "data:$HOME/.config/beets" \
	-e HOME \
	mariolopjr/beets \
	beet <subcommand>
```

Or, if you'd like to create an alias that allows beets to be ran anywhere, try the following:  

Run once
```console
```

Run whenever you want to use beets
```console
```
