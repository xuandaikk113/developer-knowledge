# Sự khác nhau giữa DTO và Entity
Entities/Models/Schemas are classes defined on the ORM (TypeORM, Mongoose, Prisma etc) level. The ORM will use the defined schemas to communicate with your database.

DTOs are classes defined on the controller and service level, where the logic happens. ORM doesn’t use the DTOs to communicate with your database.

So, DTOs help you to validate, parse, change data before the data is passed and mapped further to the defined Entity/Model/Schema and passed to database.


## Flow
Request Body -> DTO -> Entity/Model/Schema