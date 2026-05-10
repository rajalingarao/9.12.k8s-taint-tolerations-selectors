

\# Important Points on taint and tolerations and affinity

All affinity and anti-affinity rules require labels (node or pod).

Taints and tolerations are the only scheduling mechanism that does not rely on labels.









1\. Taints and Tolerations

What it is



Taint → applied on node



Toleration → applied on pod



Purpose: node repels pods unless they tolerate it



Example



Taint the node



kubectl taint node node1 env=prod:NoSchedule





Pod with toleration



spec:

&nbsp; tolerations:

&nbsp; - key: "env"

&nbsp;   operator: "Equal"

&nbsp;   value: "prod"

&nbsp;   effect: "NoSchedule"



Labels needed?



❌ NO — taints/tolerations do not use labels.



2\. Node Affinity – Required

Meaning



Pod must run on specific nodes.



Example



Label the node



kubectl label node node1 disktype=ssd





Pod spec



affinity:

&nbsp; nodeAffinity:

&nbsp;   requiredDuringSchedulingIgnoredDuringExecution:

&nbsp;     nodeSelectorTerms:

&nbsp;     - matchExpressions:

&nbsp;       - key: disktype

&nbsp;         operator: In

&nbsp;         values:

&nbsp;         - ssd



Labels needed?



✅ YES — uses node labels



3\. Node Affinity – Preferred

Meaning



Pod tries to run on matching nodes but won’t fail if not available.



affinity:

&nbsp; nodeAffinity:

&nbsp;   preferredDuringSchedulingIgnoredDuringExecution:

&nbsp;   - weight: 1

&nbsp;     preference:

&nbsp;       matchExpressions:

&nbsp;       - key: disktype

&nbsp;         operator: In

&nbsp;         values:

&nbsp;         - ssd



Labels needed?



✅ YES — uses node labels



4\. Node Anti-Affinity – Required

Meaning



Pod must NOT run on certain nodes.



Example

affinity:

&nbsp; nodeAffinity:

&nbsp;   requiredDuringSchedulingIgnoredDuringExecution:

&nbsp;     nodeSelectorTerms:

&nbsp;     - matchExpressions:

&nbsp;       - key: zone

&nbsp;         operator: NotIn

&nbsp;         values:

&nbsp;         - us-east-1a



Labels needed?



✅ YES — still matching node labels



5\. Node Anti-Affinity – Preferred

Meaning



Pod avoids certain nodes if possible.



affinity:

&nbsp; nodeAffinity:

&nbsp;   preferredDuringSchedulingIgnoredDuringExecution:

&nbsp;   - weight: 10

&nbsp;     preference:

&nbsp;       matchExpressions:

&nbsp;       - key: zone

&nbsp;         operator: NotIn

&nbsp;         values:

&nbsp;         - us-east-1a



Labels needed?



✅ YES



6\. Pod Affinity

Meaning



Pod wants to be scheduled near other pods.



Example



Run app pod on same node as database pod.



Database pod label



labels:

&nbsp; app: database





App pod affinity



affinity:

&nbsp; podAffinity:

&nbsp;   requiredDuringSchedulingIgnoredDuringExecution:

&nbsp;   - labelSelector:

&nbsp;       matchLabels:

&nbsp;         app: database

&nbsp;     topologyKey: kubernetes.io/hostname



Labels needed?



✅ YES — uses pod labels



7\. Pod Anti-Affinity

Meaning



Pods should not be placed together (high availability).



Example



Do not place two web pods on same node.



Web pod label



labels:

&nbsp; app: web





Anti-affinity rule



affinity:

&nbsp; podAntiAffinity:

&nbsp;   requiredDuringSchedulingIgnoredDuringExecution:

&nbsp;   - labelSelector:

&nbsp;       matchLabels:

&nbsp;         app: web

&nbsp;     topologyKey: kubernetes.io/hostname



Labels needed?



✅ YES — uses pod labels



Final Summary Table 🧠

Concept	Labels required?	Label type

Taints \& Tolerations	❌ No	N/A

Node Affinity (required)	✅ Yes	Node labels

Node Affinity (preferred)	✅ Yes	Node labels

Node Anti-Affinity (required)	✅ Yes	Node labels

Node Anti-Affinity (preferred)	✅ Yes	Node labels

Pod Affinity	✅ Yes	Pod labels

Pod Anti-Affinity	✅ Yes	Pod labels

One-line takeaway (perfect for interviews)



All affinity and anti-affinity rules require labels (node or pod).

Taints and tolerations are the only scheduling mechanism that does not rely on labels.





