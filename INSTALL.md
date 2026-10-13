# Install

New installs should use [route-summarization-rs](https://github.com/nabbi/route-summarization-rs)
instead; see its [deployment guide](https://github.com/nabbi/route-summarization-rs/blob/master/docs/deployment.md).

YMMV, below is an option for placing aggregateCIDR.pl into your default PATH

```shell
cd /usr/local/bin
git clone https://github.com/nabbi/route-summarization
ln -s route-summarization/aggregateCIDR.pl  aggregateCIDR.pl
```
