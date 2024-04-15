```mermaid 
---
title: User Management class diagram
---
classDiagram
  namespace coreapi_controller_v1 {
    class UserController {
      - UserService userService
      + get(userId) User
      + create(createUserCommand) User
      + update(userId, updateUserCommand) User
      + delete(userId) void 
    }

    class CreateUserCommand {
      - String username
      - String password
      - String fullName
      - String email
      - Role role
    }

    class UpdateUserCommand {
      - String oldPassword
      - String password
      - String fullName
      - String email
    }
  }

  namespace coreapi_service {
    class UserService {
      - UserPersistenceService userPersistenceService
      - UserValidationService userValidationService
      + findById(userId) User
      + create(userToCreate) User
      + update(userId, updateUserCommand) User
      + delete(userId) void
    }

    class UserPersistenceService {
      - UserRepository userRepository
      - PasswordEncoder passwordEncoder
      + findById(id) User
      + create(userToCreate) User
      + update(state) User
      + delete(id) void
      - encodePassword(user) void
    }

    class UserValidationService {
      - AuthenticationFacade authenticationFacade
      - PasswordEncoder passwordEncoder
      - UserPersistenceService userPersistenceService
      + authorizeGet(userId) void
      + validateCreate(userToCreate) void
      + validateUpdateCommand(toUpdate, updateUserCommand) void
      + authorizeUpdate(user) void
      + authorizeDelete(user) void
    }
  }

  namespace coremodel {
    class User {
      - Long id
      - String username
      - String fullName
      - String email
      - String password
      - Role role
    }

    class Role {
      <<Enumeration>>
      ADMIN
      SUPERVISOR
    }
  }

  namespace integration_db_repository {
    class UserRepository {
      <<interface>>
      UserEntity findByUsername(username)
      UserEntity findByEmail(email)
    }
  }

  namespace integration_db_entity {
    class UserEntity {
      - Long id
      - String username
      - String fullName
      - String email
      - String password
      - Role role
    }
  }

  UserController ..> CreateUserCommand
  UserController ..> UpdateUserCommand
  UserController ..> UserService
  UserController ..> User
  UserService ..> UserPersistenceService
  UserService ..> User
  UserService ..> UserValidationService
  UserService ..> User
  UserPersistenceService ..> UserRepository
  UserPersistenceService ..> User
  UserRepository ..> UserEntity
  CreateUserCommand --> Role
  User --> Role
  UserEntity --> Role
```