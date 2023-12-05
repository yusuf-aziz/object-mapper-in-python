## Why do we need an Object-Mapper?

In large applications, data is often represented differently in different parts of the system. This can make it difficult to pass data between different layers of the application.

Manually writing the code to convert data between different formats can be time-consuming and error-prone. This code is often called "boilerplate code" because it is repetitive and doesn't add any real value to the application.

This boilerplate code would be acceptable to implement once if our object or payload just requires a few fields. But if we have an object that can accept more than 20 to 30 fields this code quickly becomes cumbersome.

### ObjectMapper Introduction

A Python library called object-mapper (docs). It helps us easily and transparently construct objects between application layers (data layer, service layer, and view). In order to make object mapping simpler, it automatically decides how one object model maps to another without creating boilerplate code to convert DTOs into entities and vice versa.

As an example of what this library can do, consider the following UserEntity:

```
from dataclasses import dataclass
from typing import Optional

@dataclass
class UserEntity:

    first_name: Optional[str] = None
    last_name: Optional[str] = None
    email: Optional[str] = None
    password: Optional[str] = None
```

And let’s say we want to expose just the first_name and email. So we will create a DTO like this.

```
from dataclasses import dataclass
from typing import Optional

@dataclass
class UserDTO:

    name: Optional[str] = None
    emailId: Optional[str] = None
```

And then call ObjectMapper as follows:
```
from mapper.object_mapper import ObjectMapper

# define mappings
mapping = {
 "name":     lambda entity: entity.first_name,
 'emailId':  lambda entity: entity.email
}

mapper = ObjectMapper()
mapper.create_map(UserEntity, UserDTO, mapping)

user_entity = UserEntity("Yusuf", "Malkan", "yusuf.aziz@gmail.com", 'xyz123')
# get a instance of UserDTO
user_dto = mapper.map(user_entity, UserDTO)
```
**We did it! We reached our goal of hiding certain information by deciding what parts we want to show. We did this by using ObjectMapper.map.**
