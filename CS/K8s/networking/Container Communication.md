Communication between containers inside a [[Pod]] has to be aware of what ports are used by the other container. Given that typically there will be only one container per [[Pod]], this should not be a problem.

But how do containers between different Pods communicate?