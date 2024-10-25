#flashcards/kubernetes/cka/scheduling

- A combination of [[2 - Taints & Tolerations ✅|taints & tolerations]] and [[4 - Node Affinity ✅|nodeAffinity]] rules can be used together to completely dedicate [[7 - Pods ✅|pods]] to specific [[0 - Core Concepts Intro ✅|nodes]] 

- [[2 - Taints & Tolerations ✅|taints & tolerations]] can restrict [[7 - Pods ✅|pods]] from being placed on [[0 - Core Concepts Intro ✅|nodes]] but doesn't guarantee the [[7 - Pods ✅|pods]] will be placed on [[0 - Core Concepts Intro ✅|nodes]]

- [[3 - Node Selectors ✅|nodeSelector]] and [[4 - Node Affinity ✅|nodeAffinity]] rules guarantee [[7 - Pods ✅|pods]] will be placed on particular [[0 - Core Concepts Intro ✅|nodes]] but do not restrict [[7 - Pods ✅|pods]] from being placed on [[0 - Core Concepts Intro ✅|nodes]]