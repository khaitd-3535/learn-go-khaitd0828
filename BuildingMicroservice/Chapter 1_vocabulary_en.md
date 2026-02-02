| Term | Definition |
| :--- | :--- |
| **Microservices** | An architectural style that structures an application as a collection of services that are highly maintainable and testable, loosely coupled, independently deployable, organized around business capabilities. |
| **Independently Releasable** | The ability to deploy/release a specific service or component without needing to deploy the entire system or coordinate with other teams. |
| **Business Domain** | The specific subject area or sphere of knowledge and activity that the software is intended to support (e.g., Inventory, Billing, Shipping). |
| **SOA (Service-Oriented Architecture)** | An architectural style where software components provide services to other components via a communication protocol over a network. |
| **Technology Agnostic** | The philosophy that the architecture does not impose a specific technology stack; each service can use the best tool for the job. |
| **Black Box** | A system or component whose internal workings are hidden from the user/consumer; only inputs and outputs are visible. |
| **Network Endpoints** | A point of interaction on a network (e.g., a URL for an API) where a service can be accessed. |
| **Shared Database** | An anti-pattern in microservices where multiple services directly access the same database schema, leading to tight coupling. |
| **Information Hiding** | A design principle of hiding the implementation details of a component and exposing only a stable interface. |
| **Loose Coupling** | A state where system components have little knowledge of (or dependency on) the definitions of other separate components. |
| **Strong Cohesion** | A measure of the strength of relationship between the functionality within a single module/service. High cohesion means related functions are kept together. |
| **Hexagonal Architecture** | (Also known as Ports and Adapters) An architectural pattern that allows an application to be equally driven by users, programs, automated tests, or scripts, and to be developed and tested in isolation from its eventual run-time devices and databases. |
| **Backward-incompatible** | A change that breaks existing clients or consumers because the interface has changed in a way they do not expect. |

---
| Term | Definition |
| :--- | :--- |
| **Monolithic Application** | An application built as a single, large, unified unit. Changes often require rebuilding and deploying the entire application, leading to slower development cycles. |
| **SOAP (Simple Object Access Protocol)** | A standardized, XML-based messaging protocol for exchanging structured information in the implementation of web services. Often considered more heavyweight and complex than alternatives like REST. |
| **Vendor Middleware** | Third-party software that provides services to applications beyond those available from the operating system. In SOA, it was often used for service communication, orchestration, and monitoring. |
| **Service Granularity** | A measure of the size and scope of a service. "Fine-grained" services are small and highly focused, while "coarse-grained" services are larger and encompass broader functionality. Finding the right granularity is a key challenge in service-oriented design. |
| **Agile Software Development** | An iterative approach to software development that emphasizes collaboration, customer feedback, and rapid releases. Methodologies like Scrum and XP are specific implementations of Agile principles. |

---
