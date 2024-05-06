
https://julien-topcu.medium.com/decoupling-your-technical-code-from-your-business-logic-with-the-hexagonal-architecture-hexarch-b4da7ba62079

like an onion/layered hexagon, put your domain logic in the innermost part and make it agnostic to the implementation detail.
provide interface and depend on interface to remove coupling between layer.
APIs will use the provided interface of the domain, and the domain will be provided the implementation of the interface dependency.

other example:
https://netflixtechblog.com/ready-for-changes-with-hexagonal-architecture-b315ec967749
