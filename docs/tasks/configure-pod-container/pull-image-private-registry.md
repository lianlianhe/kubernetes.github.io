<!--
---
title: Pull an Image from a Private Registry
---

{% capture overview %}

This page shows how to create a Pod that uses a Secret to pull an image from a
private Docker registry or repository.

{% endcapture %}

{% capture prerequisites %}

* {% include task-tutorial-prereqs.md %}

* To do this exercise, you need a
[Docker ID](https://docs.docker.com/docker-id/) and password.

{% endcapture %}

{% capture steps %}
-->

---
title: 从私有仓库拉取镜像
---

{% capture overview %}

这篇教程指导如何创建Pod并使用Secret从私有Docker仓库或者镜像源拉取镜像。

{% endcapture %}

{% capture prerequisites %}

* {% include task-tutorial-prereqs.md %}

* 要完成这个实验，你需要有一个
[Docker ID](https://docs.docker.com/docker-id/) 和密码。

{% endcapture %}

{% capture steps %}

<!--
## Log in to Docker

    docker login

When prompted, enter your Docker username and password.

The login process creates or updates a `config.json` file that holds an
authorization token.

View the `config.json` file:

    cat ~/.docker/config.json

The output contains a section similar to this:

    {
        "auths": {
            "https://index.docker.io/v1/": {
                "auth": "c3R...zE2"
            }
        }
    }

**Note:** If you use a Docker credentials store, you won't see that `auth` entry but a `credsStore` entry with the name of the store as value.
{: .note}
-->

## 登录到 Docker

    docker login

然后输入你Docker的用户名和密码。

登录过程会创建或者更新`config.json`文件，在这里会保存验证的口令。

查看`config.json`文件：

    cat ~/.docker/config.json

输出包含类似这么的一段：

    {
        "auths": {
            "https://index.docker.io/v1/": {
                "auth": "c3R...zE2"
            }
        }
    }

**注意:** 如果你使用了Docker保存验证信息, 你不会看到`auth`而是`credsStore`和一样的值。
{: .note}

<!--
## Create a Secret that holds your authorization token

Create a Secret named `regsecret`:

    kubectl create secret docker-registry regsecret --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>

where:

* `<your-registry-server>` is your Private Docker Registry FQDN.
* `<your-name>` is your Docker username.
* `<your-pword>` is your Docker password.
* `<your-email>` is your Docker email.
-->

## 创建一个Secret来保存你的验证口令

创建一个名为`regsecret`的Secret:

    kubectl create secret docker-registry regsecret --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>

在这里:

* `<your-registry-server>` 是你的私有Docker仓库的FQDN.
* `<your-name>` 是你的Docker用户名.
* `<your-pword>` 是你的Docker密码.
* `<your-email>` 是你的Docker邮箱地址.

<!--
## Understanding your Secret

To understand what's in the Secret you just created, start by viewing the
Secret in YAML format:

    kubectl get secret regsecret --output=yaml

The output is similar to this:

    apiVersion: v1
    data:
      .dockercfg: eyJodHRwczovL2luZGV4L ... J0QUl6RTIifX0=
    kind: Secret
    metadata:
      ...
      name: regsecret
      ...
    type: kubernetes.io/dockercfg
-->

## 理解你的 Secret

想要知道你刚刚创建的 Secret 是什么，可以先看看 YAML 格式的 Secret：

    kubectl get secret regsecret --output=yaml

输出类似这样:

    apiVersion: v1
    data:
      .dockercfg: eyJodHRwczovL2luZGV4L ... J0QUl6RTIifX0=
    kind: Secret
    metadata:
      ...
      name: regsecret
      ...
    type: kubernetes.io/dockercfg

<!--
The value of the `.dockercfg` field is a base64 representation of your secret data.

Copy the base64 representation of the secret data into a file named `secret64`.

**Important**: Make sure there are no line breaks in your `secret64` file.

To understand what is in the `.dockercfg` field, convert the secret data to a
readable format:

    base64 -d secret64

The output is similar to this:

    {"yourprivateregistry.com":{"username":"janedoe","password":"xxxxxxxxxxx","email":"jdoe@example.com","auth":"c3R...zE2"}}

Notice that the secret data contains the authorization token from your
`config.json` file.
-->

在这里`.dockercfg`的值是一个base64加密的数据。

把这一串base64加密的数据复制赋值给`secret64`.

**注意**: 确保`secret64`的值没有分割是完整的一行.

想知道`.dockercfg`的值是什么意思，只要将数据转换成我们可读的格式即可：

    base64 -d secret64

输出类似这样:

    {"yourprivateregistry.com":{"username":"janedoe","password":"xxxxxxxxxxx","email":"jdoe@example.com","auth":"c3R...zE2"}}

其实 secret 数据包含的就是你的`config.json`文件的验证口令。

<!--
## Create a Pod that uses your Secret

Here is a configuration file for a Pod that needs access to your secret data:

{% include code.html language="yaml" file="private-reg-pod.yaml" ghlink="/docs/tasks/configure-pod-container/private-reg-pod.yaml" %}

Copy the contents of `private-reg-pod.yaml` to your own file named
`my-private-reg-pod.yaml`. In your file, replace `<your-private-image>` with
the path to an image in a private repository.

Example Docker Hub private image:

    janedoe/jdoe-private:v1

To pull the image from the private repository, Kubernetes needs credentials. The
 `imagePullSecrets` field in the configuration file specifies that Kubernetes
 should get the credentials from a Secret named
`regsecret`.

Create a Pod that uses your Secret, and verify that the Pod is running:

    kubectl create -f my-private-reg-pod.yaml
    kubectl get pod private-reg
-->

## 使用你的 Secret 创建Pod

下面是一个需要访问你的 Secrete　数据的　Pod　配置文件：

{% include code.html language="yaml" file="private-reg-pod.yaml" ghlink="/docs/tasks/configure-pod-container/private-reg-pod.yaml" %}

复制`private-reg-pod.yaml` 的内容到你的名为`my-private-reg-pod.yaml`的文件.
在这个文件里，覆盖`<your-private-image>`为私有仓库的镜像路径。

比如　Docker　Hub　的私有镜像:

    janedoe/jdoe-private:v1

要从私有仓库拉取镜像，Kubernetes　需要验证信息。配置文件里的`imagePullSecrets`
就是　Kubernetes 从名为`regsecret`获取验证信息的 Secret 名字。

创建 Pod 来使用你的 Secret, 并验证 Pod 是否运行：

    kubectl create -f my-private-reg-pod.yaml
    kubectl get pod private-reg

{% endcapture %}

{% capture whatsnext %}

* 了解更多关于[Secrets](/docs/concepts/configuration/secret/).
* 了解更多关于[使用私有仓库](/docs/concepts/containers/images/#using-a-private-registry).
* 阅读 [kubectl 创建 secret docker-registry](/docs/user-guide/kubectl/v1.6/#-em-secret-docker-registry-em-).
* 阅读 [Secret](/docs/api-reference/{{page.version}}/#secret-v1-core)
* 在这个文件[PodSpec](/docs/api-reference/{{page.version}}/#podspec-v1-core)查看 `imagePullSecrets` 内容。

{% endcapture %}

{% include templates/task.md %}
