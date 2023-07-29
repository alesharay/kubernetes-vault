- Assume the goal is to protect the DB pod in the web server - API server - DB server setup so that it doesn't allow access from any other pods except the API server pod

- By default, Kubernetes allows all communications from all pods to all destinations

- Consider the web server - API server - DB server setup: as a first step, we want to block out everything going in and out of the DB pod

- Create a network policy with the pod that we want to protect by creating a NetworkPolicy object

- Give it a name that represents what it's for

- *Remember: network policies are created using labels and selectors

- To add labels and selectors to the NetworkPolicy object, add a podSelector property with a matchLabel field under it specifying the label on the pod

- This associates the network policy with the pod and blocks out all traffic

![[developing-1.png]]

- After blocking out all traffic to the pod, we need to choose which pod needs to be able to access the pod and on which port

- It is necessary to decide what type of policies should be defined on the network policy object to meet necessary requirements

- Ingress, egress or both

- From the DB pod perspective, we want to allow traffic from the API server pod

- This is incoming traffic so we need an ingress network policy

![[developing-2.png]]

- Once you allow incoming traffic, the response or reply to that traffic is allowed back automatically

- Thus a separate rule is not needed

- When deciding on what type of rule needs to be created, you only need to be concerned about the direction in which the requests originate

- This is denoted by the straight line in the image

![[developing-3.png]]

- For an ingress policy, even though response traffic is still allowed through, new outgoing traffic (like a call from the DB to the API) can't

- A single network policy can have an ingress type, and egress type, or both in the case where a pod wants to allow incoming connections as well as wants to make external calls (like an API)

- Once you decide on the policy type, the next step is to define the specifics of that policy

- If the type is ingress, create a section called ingress within which we specify multiple rules

- Each rule for a policy type has a from and ports property

- The from property defines the source of traffic that is allowed to pass through to the specified pod

- For the from property, we use a podSelector and provide the labels of the API pod

![[developing-4.png]]

- The ports property defines what port on the pod the traffic is allowed to go to

![[developing-5.png]]

- If there are multiple pods (ie. In different namespaces) that need access to the specified pod, the created network policy allows any pod in any namespace with matchLabels to reach that pod

- If we only want to allow a pod in a specific namespace to reach the specified pod, even though other pods have matching labels, we add a new selector called the namespaceSelector on the same rule as the podSelector

- Here we match labels again using the namespace label which should be specified on the namespace first

![[developing-6.png]]

- The namespaceSelector helps in defining from what namespace traffic is allowed to reach the specified pod

- If you only have the namespaceSelector and not the podSelector, all pods within the specified namespace will be allowed to reach the specified pod

- Pods from outside of this namespace will not be allowed to go through

- If we have a server, that is not apart of the Kubernetes cluster, that we want to be able to access the specified pod, the current podSelector and namespaceSelector won't work

- It is possible to configure network policies to allow traffic from a specific IP address by adding an ipBlock property.

- This will have the CIDR property under it with the IP CIDR

![[developing-7.png]]

- The podSelector, namespaceSelector, and ipBlock properties are all able to be used in both the from and to sections of the network policy object

- podSelectors select pods by labels

- namespaceSelectors select namespaces by labels

- ipBlock selectors select IP address ranges

- The podSelector, namespaceSelector, and ipBlock properties can be passed in separately or together as part of a single rule

- The multiple rules work like OR operators thus traffic for meeting any of these criteria will be allowed to pass through

![[developing-8.png]]

- When there are multiple selectors in the same rule, all traffic must meet the requirements of all selectors in the rule, like an AND operation

![[developing-9.png]]

- It's important to understand the possibilities of rule configuration

- There can be both ingress and egress types on the same network policy

- Just like with ingress, egress needs to be added to the policyTypes section and there needs to be an egress section

- To define the specifics of the egress policy type, and the to (instead of from) and ports properties

- In the ports section, specify the port for which to send traffic

![[developing-10.png]]

### Practice Problems

- How many network policies do you see in the environment?

		kubectl get netpol

	(jot down the name of the network policy)

- Which pod is the network policy applied on?

		kubectl describe  netpol <name of previous network policy>

	(jot down the name of the podSelector matchLabel)

- What type of traffic is Network Policy configured to handle?

		kubectl describe  netpol <name of previous network policy>

	(jot down the policy type)