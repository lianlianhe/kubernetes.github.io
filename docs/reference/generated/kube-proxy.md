---
title: kube-proxy
notitle: true
---
---
标题: kube-proxy
无标题：trye
cn-approvers:
- lianlianhe
---
## kube-proxy


<!--
### Synopsis
-->
### 摘要

<!--
The Kubernetes network proxy runs on each node. This
reflects services as defined in the Kubernetes API on each node and can do simple
TCP,UDP stream forwarding or round robin TCP,UDP forwarding across a set of backends.
Service cluster ips and ports are currently found through Docker-links-compatible
environment variables specifying ports opened by the service proxy. There is an optional
addon that provides cluster DNS for these cluster IPs. The user must create a service
with the apiserver API to configure the proxy.
-->
kubernetes网络代理运行在每个节点上。这个
反映在每个节点上的Kubernetes API中定义的服务上，并且可以实现简单的
TCP，UDP流转发或TCP轮询，UDP跨越一组后端的转发。
服务集群ip和端口目前通过环境变量Docker-links-compatible
指定的由服务代理打开的端口来发现。 这是一个可选的
为这些群集IP提供群集DNS的插件。 用户必须创建一个服务
用apiserver API配置代理。

```
kube-proxy
```
<!--
### Options
-->
### 参数

<!--
```
      --azure-container-registry-config string       Path to the file container Azure container registry configuration information.
      --bind-address ip                              The IP address for the proxy server to serve on (set to 0.0.0.0 for all interfaces) (default 0.0.0.0)
      --cleanup                                      If true cleanup iptables and ipvs rules and exit.
      --cluster-cidr string                          The CIDR range of pods in the cluster. When configured, traffic sent to a Service cluster IP from outside this range will be masqueraded and traffic sent from pods to an external LoadBalancer IP will be directed to the respective cluster IP instead
      --config string                                The path to the configuration file.
      --config-sync-period duration                  How often configuration from the apiserver is refreshed.  Must be greater than 0. (default 15m0s)
      --conntrack-max-per-core int32                 Maximum number of NAT connections to track per CPU core (0 to leave the limit as-is and ignore conntrack-min). (default 32768)
      --conntrack-min int32                          Minimum number of conntrack entries to allocate, regardless of conntrack-max-per-core (set conntrack-max-per-core=0 to leave the limit as-is). (default 131072)
      --conntrack-tcp-timeout-close-wait duration    NAT timeout for TCP connections in the CLOSE_WAIT state (default 1h0m0s)
      --conntrack-tcp-timeout-established duration   Idle timeout for established TCP connections (0 to leave as-is) (default 24h0m0s)
      --feature-gates mapStringBool                  A set of key=value pairs that describe feature gates for alpha/experimental features. Options are:
APIListChunking=true|false (ALPHA - default=false)
APIResponseCompression=true|false (ALPHA - default=false)
Accelerators=true|false (ALPHA - default=false)
AdvancedAuditing=true|false (BETA - default=true)
AllAlpha=true|false (ALPHA - default=false)
AllowExtTrafficLocalEndpoints=true|false (default=true)
AppArmor=true|false (BETA - default=true)
CPUManager=true|false (ALPHA - default=false)
CustomResourceValidation=true|false (ALPHA - default=false)
DebugContainers=true|false (ALPHA - default=false)
DevicePlugins=true|false (ALPHA - default=false)
DynamicKubeletConfig=true|false (ALPHA - default=false)
EnableEquivalenceClassCache=true|false (ALPHA - default=false)
ExpandPersistentVolumes=true|false (ALPHA - default=false)
ExperimentalCriticalPodAnnotation=true|false (ALPHA - default=false)
ExperimentalHostUserNamespaceDefaulting=true|false (BETA - default=false)
HugePages=true|false (ALPHA - default=false)
Initializers=true|false (ALPHA - default=false)
KubeletConfigFile=true|false (ALPHA - default=false)
LocalStorageCapacityIsolation=true|false (ALPHA - default=false)
MountPropagation=true|false (ALPHA - default=false)
PersistentLocalVolumes=true|false (ALPHA - default=false)
PodPriority=true|false (ALPHA - default=false)
RotateKubeletClientCertificate=true|false (BETA - default=true)
RotateKubeletServerCertificate=true|false (ALPHA - default=false)
StreamingProxyRedirects=true|false (BETA - default=true)
SupportIPVSProxyMode=true|false (ALPHA - default=false)
TaintBasedEvictions=true|false (ALPHA - default=false)
TaintNodesByCondition=true|false (ALPHA - default=false)
      --google-json-key string                       The Google Cloud Platform Service Account JSON Key to use for authentication.
      --healthz-bind-address ip                      The IP address and port for the health check server to serve on (set to 0.0.0.0 for all interfaces) (default 0.0.0.0:10256)
      --healthz-port int32                           The port to bind the health check server. Use 0 to disable. (default 10256)
      --hostname-override string                     If non-empty, will use this string as identification instead of the actual hostname.
      --iptables-masquerade-bit int32                If using the pure iptables proxy, the bit of the fwmark space to mark packets requiring SNAT with.  Must be within the range [0, 31]. (default 14)
      --iptables-min-sync-period duration            The minimum interval of how often the iptables rules can be refreshed as endpoints and services change (e.g. '5s', '1m', '2h22m').
      --iptables-sync-period duration                The maximum interval of how often iptables rules are refreshed (e.g. '5s', '1m', '2h22m').  Must be greater than 0. (default 30s)
      --ipvs-min-sync-period duration                The minimum interval of how often the ipvs rules can be refreshed as endpoints and services change (e.g. '5s', '1m', '2h22m').
      --ipvs-scheduler string                        The ipvs scheduler type when proxy mode is ipvs
      --ipvs-sync-period duration                    The maximum interval of how often ipvs rules are refreshed (e.g. '5s', '1m', '2h22m').  Must be greater than 0.
      --kube-api-burst int                           Burst to use while talking with kubernetes apiserver (default 10)
      --kube-api-content-type string                 Content type of requests sent to apiserver. (default "application/vnd.kubernetes.protobuf")
      --kube-api-qps float32                         QPS to use while talking with kubernetes apiserver (default 5)
      --kubeconfig string                            Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --masquerade-all                               If using the pure iptables proxy, SNAT all traffic sent via Service cluster IPs (this not commonly needed)
      --master string                                The address of the Kubernetes API server (overrides any value in kubeconfig)
      --metrics-bind-address ip                      The IP address and port for the metrics server to serve on (set to 0.0.0.0 for all interfaces) (default 127.0.0.1:10249)
      --oom-score-adj int32                          The oom-score-adj value for kube-proxy process. Values must be within the range [-1000, 1000] (default -999)
      --profiling                                    If true enables profiling via web interface on /debug/pprof handler.
      --proxy-mode ProxyMode                         Which proxy mode to use: 'userspace' (older) or 'iptables' (faster) or 'ipvs'(experimental)'. If blank, use the best-available proxy (currently iptables).  If the iptables proxy is selected, regardless of how, but the system's kernel or iptables versions are insufficient, this always falls back to the userspace proxy.
      --proxy-port-range port-range                  Range of host ports (beginPort-endPort, inclusive) that may be consumed in order to proxy service traffic. If unspecified (0-0) then ports will be randomly chosen.
      --udp-timeout duration                         How long an idle UDP connection will be kept open (e.g. '250ms', '2s').  Must be greater than 0. Only applicable for proxy-mode=userspace (default 250ms)
      --version version[=true]                       Print version information and quit
      --write-config-to string                       If set, write the default configuration values to this file and exit.
```
-->

```
      --azure-container-registry-config string       Azure容器注册配置信息文件的路径。
      --bind-address ip                              代理服务器的地址（所有接口设置为0.0.0.0）（默认为 0.0.0.0）
      --cleanup                                      如果为真清理iptables和ipvs规则并推出。
      --cluster-cidr string                          集群中的POD的CIDR地址范围。配置后，从该范围之外发送到服务群集IP的流量将被伪装，并且从pod发送到外部LoadBalancer IP的流量将被定向到相应的群集IP。
      --config string                                配置文件路径。The path to the configuration file.
      --config-sync-period duration                  刷新apisever的配置的频率。 必须大于0。（默认值 15m0s）
      --conntrack-max-per-core int32                 每个CPU内核可追踪的最大NAT链接数（设置为0时参数conntrack-min不启用）。（默认值 32768）
      --conntrack-min int32                          可分配的最小链接跟踪条目，可忽略参数conntrack-max-per-core（设置 conntrack-max-per-core=0 该参数不启用）。（默认值 131072）
      --conntrack-tcp-timeout-close-wait duration    CLOSE_WAIT状态下的TCP链接NAT超时时间（默认 1h0m0s）
      --conntrack-tcp-timeout-established duration   TCP链接的空闲超时时间 (0 为保留当前值) (默认值 24h0m0s)
      --feature-gates mapStringBool                  一组Alpha/实验性的特性键值对描述。参数如下：
APIListChunking=true|false (ALPHA - 默认值=false)
APIResponseCompression=true|false (ALPHA - 默认值=false)
Accelerators=true|false (ALPHA - 默认值=false)
AdvancedAuditing=true|false (BETA - 默认值=true)
AllAlpha=true|false (ALPHA - 默认值=false)
AllowExtTrafficLocalEndpoints=true|false (默认值=true)
AppArmor=true|false (BETA - 默认值=true)
CPUManager=true|false (ALPHA - 默认值=false)
CustomResourceValidation=true|false (ALPHA - 默认值=false)
DebugContainers=true|false (ALPHA - 默认值=false)
DevicePlugins=true|false (ALPHA - 默认值=false)
DynamicKubeletConfig=true|false (ALPHA - 默认值=false)
EnableEquivalenceClassCache=true|false (ALPHA - 默认值=false)
ExpandPersistentVolumes=true|false (ALPHA - 默认值=false)
ExperimentalCriticalPodAnnotation=true|false (ALPHA - 默认值=false)
ExperimentalHostUserNamespaceDefaulting=true|false (BETA - 默认值=false)
HugePages=true|false (ALPHA - 默认值=false)
Initializers=true|false (ALPHA - 默认值=false)
KubeletConfigFile=true|false (ALPHA - 默认值=false)
LocalStorageCapacityIsolation=true|false (ALPHA - 默认值=false)
MountPropagation=true|false (ALPHA - 默认值=false)
PersistentLocalVolumes=true|false (ALPHA - 默认值=false)
PodPriority=true|false (ALPHA - 默认值=false)
RotateKubeletClientCertificate=true|false (BETA - 默认值=true)
RotateKubeletServerCertificate=true|false (ALPHA - 默认值=false)
StreamingProxyRedirects=true|false (BETA - 默认值=true)
SupportIPVSProxyMode=true|false (ALPHA - 默认值=false)
TaintBasedEvictions=true|false (ALPHA - 默认值=false)
TaintNodesByCondition=true|false (ALPHA - 默认值=false)
      --google-json-key string                       Google云端平台服务帐户用于身份验证的JSON密钥。
      --healthz-bind-address ip                      要运行健康检查服务器的IP地址和端口（对于所有接口，设置为0.0.0.0）（默认为0.0.0.0:10256）
      --healthz-port int32                           可健康检查服务器的绑定端口。0为不启用。（默认值 10256）
      --hostname-override string                     如果指定，将会使用这个字符串代替现有实际使用的主机名。
      --iptables-masquerade-bit int32                当只使用iptable代理时，则fwmark标记的每bit需要使用SNAT标识。范围必须在[0.31].（默认值 14）
      --iptables-min-sync-period duration            当endpoints和服务变更后最小的的iptables规则刷新间隔。(例如： '5s', '1m', '2h22m')。
      --iptables-sync-period duration                当endpoints和服务变更后最大的的iptables规则刷新间隔。(例如： '5s', '1m', '2h22m')。必须大于0.（默认值 30s）。
      --ipvs-min-sync-period duration                当endpoints和服务变更后最小的的ipvs规则刷新间隔。(例如： '5s', '1m', '2h22m')。
      --ipvs-scheduler string                        代理为ipvs时的ipvs的调度类型。
      --ipvs-sync-period duration                    ipvs规则的最大刷新间隔。(例如. '5s', '1m', '2h22m').必须大于0。
      --kube-api-burst int                           与kubernetes apiserver的激增交互数。（默认值 10）
      --kube-api-content-type string                 发送给apiserver的请求的内容类型。 （默认“application / vnd.kubernetes.protobuf”）
      --kube-api-qps float32                         与kubernetes apiserver交互时的QPS。（默认值 5）
      --kubeconfig string                            具有授权信息的kubeconfig文件的路径（master的由master标志设置）。
      --masquerade-all                               如果只使用iptables代理，SNAT通过服务集群IP发送的所有流量（这通常是不必要的）
      --master string                                Kubernetes API 服务器地址 (覆盖 kubeconfig 中的值)
      --metrics-bind-address ip                      metrics server的IP地址和端口(对所有接口设置为0.0.0.0) (默认值  127.0.0.1:10249)
      --oom-score-adj int32                          kube-proxy进程的oom-score-adj的设置值。 该值范围必须在 [-1000, 1000]中。 (默认值 -999)
      --profiling                                    如果允许 profiling 直接通过 /debug/pprof 接口处理.
      --proxy-mode ProxyMode                         使用哪种代理模式：“用户空间”（旧）或“iptables”（更快）或“ipvs（实验）”。 如果空白，使用最佳可用代理（当前是iptables）。 如果选择iptables代理，但系统的内核或iptables版本不符，会总是回退到用户空间代理。
      --proxy-port-range port-range                  代理服务流量使用的主机端口范围(包含beginPort-endPort) . 未指定情况下(0-0)端口将被随机指定。
      --udp-timeout duration                         UDP链接的保持打开时间 (例如. '250ms', '2s').  必须大于 0. 仅适用于proxy-mode = userspace（默认250ms）
      --version version[=true]                       打印版本信息并退出。
      --write-config-to string                       如果设置，则将默认配置值写入此文件并退出。
```

###### Auto generated by spf13/cobra on 27-Sep-2017
