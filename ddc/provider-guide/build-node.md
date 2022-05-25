# ⚒ Build Node

{% hint style="warning" %}
**Recommendation:** Use prepared docker images for starting nodes.
{% endhint %}

## Source code

DDC node’s build system requires Go 1.18

## Install Go

The build process for DDC Node requires Go 1.18 or higher. See the [Go install instructions](https://go.dev/doc/install)

## Download source code

{% tabs %}
{% tab title="CDN Node" %}
```shell
git clone https://github.com/Cerebellum-Network/ddc-cdn-node.git
```
{% endtab %}

{% tab title="Storage Node" %}
```shell
git clone https://github.com/Cerebellum-Network/ddc-storage-node.git
```
{% endtab %}
{% endtabs %}

## Build

{% tabs %}
{% tab title="Linux" %}
Run command from ddc-cdn-node and ddc-storage-node folder.

```shell
go build -a -installsuffix cgo -o application ./
```
{% endtab %}

{% tab title="Windows" %}
Run command from ddc-cdn-node and ddc-storage-node folder.

```shell
go build -a -installsuffix cgo -o application.exe ./ 
```
{% endtab %}

{% tab title="Docker Image" %}
* Run command from ddc-cdn-node folder:

```shell
docker build -t cere/ddc-cdn-node:latest .
```

* Run command from ddc-storage-node folder:

```shell
docker build -t cere/ddc-storage-node:latest .
```
{% endtab %}
{% endtabs %}
