# i.am.fog.fish 


This is the data for [my blog](https://i.am.fog.fish).

## Getting started

```
podman build -t i-am-fog-fish .
podman run -it \
  -v /mnt/macos/js/fogfish.github.io:/usr/src/app \
  -p 4000:4000 \
  i-am-fog-fish \
  bundle exec jekyll serve -H 0.0.0.0 -t
```



## License

Copyright © 2012 - 2019 Dmitry Kolesnikov, All Right Reserved.

All materials contained in the `_posts` folder are copyrighted and might be used for educational purposes only. The usage of material in other context requires a written permission.

The materials contained in the `_posts` folder may not be copied. No material may be modified, edited or taken out of the context.

The author holds rights to publish any materials contained in this document to public domain or use them in commercial environments without any prior notice.

All other directories and files are MIT Licensed.


