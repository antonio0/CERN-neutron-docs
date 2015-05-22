# Extensions

Neutron APIs can be extended in different ways based on the use case. There are 3 types of extensions:
* Resource extensions are used to introduce a new API entity
* Action extensions are used to introduce new actions on an entity besides the basic CRUD operations
* Request extensions are used to add new properties to existing entities

All active API extensions need to have a corresponding plugin that supports their functions.

In most scenarios, resource and action extensions are mapped to new service plugins that implement the API interface.

On the other hand, in order to write an implementation of a request API extension, we can create a new extension driver.

