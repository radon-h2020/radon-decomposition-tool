The *decomposition tool* aims to help software developers in finding the optimal decomposition solution for an application based on the microservices architectural style and the serverless FaaS paradigm. It is a standalone tool that follows the `TOSCA <http://www.oasis-open.org/committees/tosca>`_ standard and extends the `RADON <https://radon-h2020.eu/>`_ framework. The typical usage scenarios of the tool are envisioned as follows:

- *Architecture Decomposition*: It can be used to generate a coarse-grained or fine-grained TOSCA model from a monolithic one, which will be realized by analyzing functional dependencies among the interfaces and applying appropriate architectural design patterns.

- *Deployment Optimization*: In this scenario, it is used to obtain the optimal deployment scheme for either a platform-independent or platform-specific TOSCA model, minimizing the operating costs on the target cloud platform under the performance requirements.

- *Accuracy Enhancement*: It can also be used to enhance the accuracy of performance annotations in a TOSCA model according to runtime monitoring data so that a better decomposition or optimization result may be achieved afterwards. This enables an iterative lifecycle for developing the application.

- *Assignment/Consolidation*: Additionally, it is used to consolidate the assignment of container applications in a TOSCA model by learning from runtime experiment data, and the job inteferences among the component microservices is thus minimized on the available compute nodes.
