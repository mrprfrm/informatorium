---
authors:
  - Anton Petrov
status: note
tags:
  - python
  - database
  - extending
---

For cases when we have multiple models of the same kind, but with different sets of attributes per model and different types as a result, we may think about abstraction. However, in this case, abstract models cannot be used in queries, since there is no appropriate table in the database. Instead, we can try to implement extendable models.

## Extendable Models Definition

Let’s start by defining a base model that contains all the shared fields along with the polymorphic configuration. This setup lets child classes inherit the core structure while customizing what they need.

```python
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer

Base = declarative_base()

class Entity(Base):
    __tablename__ = "entities"
    id = Column(Integer, primary_key=True)
    entity_id = Column(Integer, nullable=False)
    entity_type = Column(Integer, nullable=False)

    __mapper_args__ = {
        "polymorphic_on": entity_type,
        "polymorphic_identity": 0,  # the default entity type is undefined
    }
```

- `polymorphic_on`: Indicates which field controls subclass selection.
- `polymorphic_identity`: Assigns a specific type identity to this class in the polymorphic hierarchy.

To enforce a structured type system, define the valid entity types using `IntEnum`:

```python
import enum

class EntityType(enum.IntEnum):
    CLIENT = 1
    EMPLOYEE = 2
```

## Extended Model Definition

Now let’s create a child model that builds on the base entity. Here, we’ll add fields specific to this type and assign the correct polymorphic identity so SQLAlchemy knows how to distinguish it:

```python
class ClientEntity(Entity):
    __tablename__ = "client_entities"
    id = Column(Integer, primary_key=True)
    name = Column(String)

    __mapper_args__ = {
        "polymorphic_identity": EntityType.CLIENT,  # Client's entity type
    }
```

## Field Aliasing

For those cases when we need to alias fields from the original model inside a child, it's not necessary to completely override these fields — SQLAlchemy provides tools to alias and customize them cleanly.

### Using `synonym`

To rename a parent field in the child model without changing database structure, use SQLAlchemy's `synonym` mapping:

```python
from sqlalchemy.orm import synonym

class ClientEntity(Entity):
    __tablename__ = "client_entities"
    id = Column(Integer, primary_key=True)
    project_id = synonym('owner_id')
    __mapper_args__ = {
        "polymorphic_identity": 1,
    }
```

### Using `hybrid_property`

For more advanced aliasing or logic encapsulation, define a `hybrid_property`:

```python
from sqlalchemy.ext.hybrid import hybrid_property

class ClientEntity(Entity):
    __tablename__ = "client_entities"
    id = Column(Integer, primary_key=True)
    __mapper_args__ = {
        "polymorphic_identity": 1,
    }

    @hybrid_property
    def client_id(self):
        return self.entity_id

    @client_id.setter
    def client_id(self, value):
        self.entity_id = value
```

> `hybrid_property` allows for complex getter/setter behavior, including computed values and logic across multiple fields or relationships.

## Insert Extended Model

When a new child record is created, SQLAlchemy automatically manages inserts into both the base and subclass tables. This behavior mimics PostgreSQL's handling of table inheritance.

```python
ce = ClientEntity(client_id=1)
session.add(ce)
session.commit()
```

Resulting schema content:

| wallet | client_wallet |             |     |     |     |
| ------ | ------------- | ----------- | --- | --- | --- |
| id     | entity_id     | entity_type | ... | id  | ... |
| 1      | 1             | 1           | ... | 1   | ... |

## TL;DR

- Use extendable models when abstract base classes can't be queried directly.
- Define polymorphic models by specifying `polymorphic_on` to control subclass resolution and assigning a unique `polymorphic_identity` to each model.
- For field aliasing, use `synonym` for direct name mapping, or `hybrid_property` when custom logic is required.
- SQLAlchemy automatically inserts matching rows into parent and child tables when persisting a subclass instance.

## References

- [SQLAlchemy ORM Inheritance Patterns](https://docs.sqlalchemy.org/en/20/orm/inheritance.html)
- [Synonym Mapped Attributes](https://docs.sqlalchemy.org/en/20/orm/mapped_attributes.html#synonyms)
- [Hybrid Properties](https://docs.sqlalchemy.org/en/20/orm/extensions/hybrid.html)
