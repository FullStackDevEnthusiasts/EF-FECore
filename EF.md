1. What is Entity Framework (EF)?
   - EF is an ORM (Object-Relational Mapper) for .NET.
   - It allows developers to work with databases using .NET objects.
   - It eliminates the need for most of the data-access code developers usually need to write.
   - EF maps the data in the database to .NET objects for CRUD operations.
   - It supports various database providers like SQL Server, SQLite, and more.

2. What is Entity Framework Core (EF Core)?
   - EF Core is a lightweight, extensible, and cross-platform version of EF.
   - It is designed to work with .NET Core applications.
   - EF Core has a more flexible architecture than EF.
   - It supports LINQ queries, change tracking, updates, and schema migrations.
   - EF Core can be used in ASP.NET Core, Xamarin, and other .NET applications.

3. What are the advantages of using EF Core?
   - It supports cross-platform development.
   - EF Core offers better performance compared to EF.
   - It provides better control over database schema migrations.
   - EF Core supports batching of statements to improve performance.
   - It has better support for asynchronous programming.

4. What is a DbContext in EF and EF Core?
   - DbContext is the primary class responsible for interacting with the database.
   - It manages the entity objects during runtime.
   - DbContext is responsible for querying the database and saving data.
   - It provides methods for performing CRUD operations.
   - DbContext also handles connection management and transaction processing.

5. How do you configure a DbContext in EF Core?
   - You configure a DbContext using the OnConfiguring method.
   - It is usually done by overriding the OnConfiguring method in your DbContext class.
   - You can also use the AddDbContext method in the Startup.cs file for ASP.NET Core applications.
   - Connection strings are typically provided via IConfiguration.
   - Options like lazy loading, logging, and caching can be configured.
   - **Example:**
     ```csharp
     protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
     {
         optionsBuilder.UseSqlServer("your_connection_string");
     }
     ```

6. What is the Code-First approach in EF?
   - Code-First allows you to create a database from your domain classes.
   - You define your entities and relationships using C# classes.
   - EF uses these classes to generate the database schema.
   - It supports migrations to update the database schema without losing data.
   - Code-First is useful for developers who prefer writing code over designing databases.
   - **Example:**
     ```csharp
     public class Product
     {
         public int Id { get; set; }
         public string Name { get; set; }
     }
     ```

7. What is the Database-First approach in EF?
   - Database-First allows you to generate entity classes from an existing database.
   - You start with an existing database schema.
   - EF creates classes that map to the database tables and relationships.
   - It is useful when working with legacy databases.
   - Changes to the database schema require updating the model.
   - **Example:**
     ```shell
     Scaffold-DbContext "your_connection_string" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
     ```

8. What is the Model-First approach in EF?
   - Model-First allows you to design the model in a designer and then generate the database schema.
   - You use the EF designer to create entities and relationships visually.
   - EF generates the code and database schema based on the model.
   - It is useful for visualizing and designing complex schemas.
   - Changes in the model require regenerating the database schema.

9. How do you perform CRUD operations in EF Core?
   - Use the DbSet<TEntity> properties of DbContext for CRUD operations.
   - Use the Add method to insert a new entity.
   - Use the Update method to modify an existing entity.
   - Use the Remove method to delete an entity.
   - Use LINQ queries for retrieving data.
   - **Example:**
     ```csharp
     var newProduct = new Product { Name = "New Product" };
     context.Products.Add(newProduct);
     context.SaveChanges();
     ```

10. What is LINQ and how is it used in EF Core?
    - LINQ stands for Language Integrated Query.
    - It allows querying of data using C# syntax.
    - LINQ queries are translated to SQL queries by EF Core.
    - You can use LINQ to query, filter, sort, and project data from the database.
    - LINQ provides compile-time syntax checking and IntelliSense support.
    - **Example:**
      ```csharp
      var products = context.Products.Where(p => p.Name.Contains("Product")).ToList();
      ```

11. What is lazy loading in EF Core?
    - Lazy loading is a way to load related data only when it is accessed.
    - It helps in reducing the initial load time and memory usage.
    - EF Core supports lazy loading through proxy classes or manual implementation.
    - Lazy loading can be enabled by installing the Microsoft.EntityFrameworkCore.Proxies package.
    - You need to configure it in the DbContext by overriding the OnConfiguring method.
    - **Example:**
      ```csharp
      optionsBuilder.UseLazyLoadingProxies().UseSqlServer("your_connection_string");
      ```

12. What is eager loading in EF Core?
    - Eager loading loads related data as part of the initial query.
    - It uses the Include method to specify related data to load.
    - Eager loading helps in reducing the number of database calls.
    - It is useful when you know you will need related data immediately.
    - Eager loading can be configured using lambda expressions or string-based paths.
    - **Example:**
      ```csharp
      var products = context.Products.Include(p => p.Category).ToList();
      ```

13. What is explicit loading in EF Core?
    - Explicit loading is a way to load related data on demand.
    - It is useful when you need related data after the initial query.
    - Use the Load method to load related data explicitly.
    - Explicit loading is more flexible than eager loading.
    - It provides control over when related data is loaded.
    - **Example:**
      ```csharp
      var product = context.Products.Find(1);
      context.Entry(product).Reference(p => p.Category).Load();
      ```

14. How do you handle concurrency in EF Core?
    - Concurrency is managed using optimistic concurrency control.
    - EF Core supports concurrency tokens to detect changes made by other users.
    - Use the [ConcurrencyCheck] attribute or IsConcurrencyToken method to mark concurrency properties.
    - Handle concurrency conflicts using try-catch blocks and resolve them manually.
    - Concurrency conflicts result in DbUpdateConcurrencyException exceptions.
    - **Example:**
      ```csharp
      public class Product
      {
          public int Id { get; set; }
          [ConcurrencyCheck]
          public string Name { get; set; }
      }
      ```

15. What are migrations in EF Core?
    - Migrations allow you to update the database schema without losing data.
    - They provide a way to version your database schema.
    - Use the Add-Migration command to create a new migration.
    - Use the Update-Database command to apply migrations to the database.
    - Migrations are useful for managing schema changes over time.
    - **Example:**
      ```shell
      Add-Migration InitialCreate
      Update-Database
      ```

16. How do you seed data in EF Core?
    - Seeding allows you to populate the database with initial data.
    - Use the OnModelCreating method in DbContext to configure seeding.
    - Use the HasData method to specify the seed data.
    - Seed data is applied during migrations or database creation.
    - Seeding is useful for providing default data or testing purposes.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>().HasData(new Product { Id = 1, Name = "Seed Product" });
      ```

17. What are value converters in EF Core?
    - Value converters allow you to transform data during reading and writing.
    - They are useful for mapping non-standard data types.
    - Use the HasConversion method in OnModelCreating to configure converters.
    - Value converters can be applied to properties of entity classes.
    - They provide flexibility in handling complex data types.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>()
          .Property(p => p.Status)
          .HasConversion<string>();
      ```

18. How do you configure relationships in EF Core?
    - Relationships are configured using the Fluent API or data annotations.
    - Use the HasOne, HasMany, WithOne, and WithMany methods for Fluent API configuration.
    - Use attributes like [ForeignKey], [InverseProperty], and [Required] for data annotations.
    - EF Core supports one-to-one, one-to-many, and many-to-many relationships.
    - Proper configuration ensures correct database schema and query generation.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>()
          .HasOne(p => p.Category)
          .WithMany(c => c.Products)
          .HasForeignKey(p => p.CategoryId);
      ```

19. What is a shadow property in EF Core?
    - Shadow properties are properties not defined in the entity class but mapped to the database.
    - They are useful for tracking additional data without modifying the entity class.
    - Use the Fluent API to configure shadow properties.
    - Shadow properties can be included in queries and change tracking.
    - They provide flexibility in managing additional metadata.
    - **

Example:**
      ```csharp
      modelBuilder.Entity<Product>().Property<DateTime>("LastUpdated");
      ```

20. How do you configure database providers in EF Core?
    - EF Core supports multiple database providers like SQL Server, SQLite, MySQL, PostgreSQL, and more.
    - Configure the provider in the DbContext's OnConfiguring method or Startup.cs in ASP.NET Core.
    - Use the appropriate NuGet package for the desired database provider.
    - Each provider may have specific configuration options.
    - Changing the provider may require adjustments to the DbContext configuration.
    - **Example:**
      ```csharp
      optionsBuilder.UseSqlite("Data Source=blog.db");
      ```

21. What is change tracking in EF Core?
    - Change tracking is the process of keeping track of changes made to entities.
    - EF Core automatically tracks changes to entities retrieved from the database.
    - The DbContext tracks the original values and current values of properties.
    - Change tracking is used to generate SQL statements for updates.
    - It supports detecting added, modified, and deleted entities.
    - **Example:**
      ```csharp
      var product = context.Products.Find(1);
      product.Name = "Updated Product";
      context.SaveChanges();
      ```

22. What is the difference between Add and Attach in EF Core?
    - Add marks an entity as Added, so it will be inserted into the database.
    - Attach marks an entity as Unchanged, so no database operation will be performed.
    - Add is used for new entities that need to be saved to the database.
    - Attach is used for existing entities that are already in the database.
    - Use Attach to bring an existing entity into the context without marking it for insertion.
    - **Example:**
      ```csharp
      context.Products.Add(new Product { Name = "New Product" }); // Adds a new product
      context.Products.Attach(existingProduct); // Attaches an existing product
      ```

23. How do you implement one-to-one relationships in EF Core?
    - Configure one-to-one relationships using the Fluent API or data annotations.
    - Use the HasOne and WithOne methods for Fluent API configuration.
    - Specify the foreign key and principal key properties if needed.
    - Ensure that one of the entities has a reference navigation property.
    - Use the [ForeignKey] and [Required] attributes for data annotations.
    - **Example:**
      ```csharp
      modelBuilder.Entity<User>()
          .HasOne(u => u.Address)
          .WithOne(a => a.User)
          .HasForeignKey<Address>(a => a.UserId);
      ```

24. How do you implement one-to-many relationships in EF Core?
    - Configure one-to-many relationships using the Fluent API or data annotations.
    - Use the HasOne and WithMany methods for Fluent API configuration.
    - Specify the foreign key property if needed.
    - Ensure that one entity has a collection navigation property.
    - Use the [ForeignKey] and [Required] attributes for data annotations.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Category>()
          .HasMany(c => c.Products)
          .WithOne(p => p.Category)
          .HasForeignKey(p => p.CategoryId);
      ```

25. How do you implement many-to-many relationships in EF Core?
    - EF Core 5.0 and later supports many-to-many relationships without a join entity.
    - Use the HasMany and WithMany methods to configure the relationship.
    - If using a join entity, configure it using HasKey and Fluent API methods.
    - Ensure that both entities have collection navigation properties.
    - Use the JoinEntity configuration to specify the linking table.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Student>()
          .HasMany(s => s.Courses)
          .WithMany(c => c.Students)
          .UsingEntity<Enrollment>(
              j => j.HasOne(e => e.Course).WithMany(c => c.Enrollments).HasForeignKey(e => e.CourseId),
              j => j.HasOne(e => e.Student).WithMany(s => s.Enrollments).HasForeignKey(e => e.StudentId));
      ```

26. What is the purpose of DbSet in EF Core?
    - DbSet represents a collection of entities of a specific type.
    - It provides methods for querying and working with the entity type.
    - DbSet<TEntity> allows CRUD operations on the entity type.
    - It is used in DbContext to define properties for each entity type.
    - DbSet is the primary way to interact with the database for a specific entity type.
    - **Example:**
      ```csharp
      public DbSet<Product> Products { get; set; }
      ```

27. How do you handle transactions in EF Core?
    - EF Core supports transactions for ensuring data integrity.
    - Use the BeginTransaction, CommitTransaction, and RollbackTransaction methods.
    - You can use the Database property of DbContext for transaction management.
    - Transactions are used to group multiple operations into a single unit of work.
    - Handle exceptions and ensure transactions are properly committed or rolled back.
    - **Example:**
      ```csharp
      using var transaction = context.Database.BeginTransaction();
      try
      {
          // Perform multiple operations
          context.SaveChanges();
          transaction.Commit();
      }
      catch
      {
          transaction.Rollback();
      }
      ```

28. How do you configure logging in EF Core?
    - Logging helps in debugging and monitoring EF Core operations.
    - Use the ILoggerFactory to configure logging in DbContext.
    - You can enable logging by configuring the DbContextOptionsBuilder.
    - Set up logging providers like Console, Debug, or custom providers.
    - Control the level of detail in logs using log levels.
    - **Example:**
      ```csharp
      var optionsBuilder = new DbContextOptionsBuilder<ApplicationDbContext>();
      optionsBuilder.UseSqlServer("your_connection_string")
                    .LogTo(Console.WriteLine, LogLevel.Information);
      ```

29. What is the purpose of the AsNoTracking method in EF Core?
    - AsNoTracking is used to query entities without tracking them.
    - It improves performance for read-only operations.
    - No change tracking means EF Core won't track changes to entities.
    - It is useful when you don't need to update the entities.
    - AsNoTracking can be applied to a query to disable tracking.
    - **Example:**
      ```csharp
      var products = context.Products.AsNoTracking().ToList();
      ```

30. What is a migration snapshot in EF Core?
    - A migration snapshot captures the current state of the model.
    - It is used to compare the model during subsequent migrations.
    - The snapshot file is generated with each migration.
    - It helps in generating migration scripts for schema changes.
    - Snapshot files are located in the Migrations folder.
    - **Example:**
      ```shell
      Add-Migration AddNewTable
      ```

31. How do you handle stored procedures in EF Core?
    - EF Core allows executing raw SQL queries and stored procedures.
    - Use FromSqlRaw or ExecuteSqlRaw methods to execute stored procedures.
    - Map the results to entity types or custom result classes.
    - Pass parameters to stored procedures using parameterized queries.
    - Handle the results using LINQ and entity mapping.
    - **Example:**
      ```csharp
      var products = context.Products.FromSqlRaw("EXEC GetProducts").ToList();
      ```

32. How do you handle raw SQL queries in EF Core?
    - EF Core supports executing raw SQL queries for custom operations.
    - Use the FromSqlRaw method for queries returning entities.
    - Use the ExecuteSqlRaw method for non-query operations.
    - Map the results to entity types or custom result classes.
    - Ensure proper parameterization to avoid SQL injection.
    - **Example:**
      ```csharp
      var products = context.Products.FromSqlRaw("SELECT * FROM Products").ToList();
      ```

33. How do you use the Fluent API in EF Core?
    - The Fluent API provides a way to configure the model using code.
    - Use the OnModelCreating method in DbContext to configure the model.
    - It allows configuring entity properties, relationships, and keys.
    - The Fluent API is more powerful and flexible than data annotations.
    - It supports complex configurations and overrides default conventions.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>()
          .Property(p => p.Name)
          .HasMaxLength(100)
          .IsRequired();
      ```

34. How do you configure composite keys in EF Core?
    - Composite keys are configured using the Fluent API.
    - Use the HasKey method to specify multiple key properties.
    - Composite keys are useful for entities with multiple primary key columns.
    - Ensure that the specified properties form a unique identifier.
    - Composite keys are configured in the OnModelCreating method.
    - **Example:**
      ```csharp
      modelBuilder.Entity<OrderDetail>()
          .HasKey(od => new { od.OrderId, od.ProductId });
      ```

35. What is database scaffolding in EF Core?
    - Database scaffolding generates entity classes and DbContext from an existing database.
    - It is used for reverse engineering the database schema into code.
    - Use the Scaffold-DbContext command to scaffold the database.
    - Scaffolding supports various database providers.
    - It helps in quickly creating a data model for existing databases.
    - **Example:**
      ```shell
      Scaffold-DbContext "your_connection_string" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
      ```

36. What is query splitting in EF Core?
    - Query splitting is a way to split a single query into multiple queries.
    - It helps in optimizing performance for complex queries.
    - EF Core 5.0 and later support query splitting for collections.
    - Use the AsSplitQuery method to enable query splitting.
    - It can reduce the impact of the N+1 query problem.
    - **Example:**
      ```csharp
      var orders = context.Orders.Include(o => o.OrderItems).AsSplitQuery().ToList();
      ```

37. How do you handle global query filters in EF Core?
    - Global query filters are applied to all queries for a specific entity type.
    - Use the HasQueryFilter method to define the filter in

 OnModelCreating.
    - Filters are useful for implementing soft deletes, multi-tenancy, etc.
    - They are automatically applied to LINQ queries for the entity type.
    - Global query filters can be overridden or disabled in specific queries.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>().HasQueryFilter(p => !p.IsDeleted);
      ```

38. How do you handle database views in EF Core?
    - EF Core allows mapping entities to database views.
    - Use the ToView method to map an entity to a view.
    - Views are useful for read-only queries and aggregations.
    - Ensure the entity class matches the view schema.
    - Views can be queried like regular tables but are read-only.
    - **Example:**
      ```csharp
      modelBuilder.Entity<ProductView>().ToView("View_ProductDetails");
      ```

39. How do you handle disconnected entities in EF Core?
    - Disconnected entities are entities not currently tracked by the DbContext.
    - Use the Attach method to bring a disconnected entity into the context.
    - Ensure that the entity's state is correctly set (Added, Modified, Deleted).
    - Handle concurrency and validation issues when updating disconnected entities.
    - Disconnected entities are common in web applications and services.
    - **Example:**
      ```csharp
      context.Products.Attach(disconnectedProduct);
      context.Entry(disconnectedProduct).State = EntityState.Modified;
      context.SaveChanges();
      ```

40. What are interceptors in EF Core?
    - Interceptors allow you to intercept and modify EF Core operations.
    - Use interceptors to handle events like query execution, command execution, etc.
    - Implement the IInterceptor interface and register it with DbContext.
    - Interceptors are useful for logging, auditing, caching, and more.
    - EF Core supports various types of interceptors like SaveChangesInterceptor, CommandInterceptor, etc.
    - **Example:**
      ```csharp
      public class LoggingInterceptor : DbCommandInterceptor
      {
          public override InterceptionResult<int> NonQueryExecuting(DbCommand command, CommandEventData eventData, InterceptionResult<int> result)
          {
              Console.WriteLine(command.CommandText);
              return base.NonQueryExecuting(command, eventData, result);
          }
      }
      ```

  41. What is shadow state in EF Core?
    - Shadow state properties are properties that do not exist in the entity class.
    - They are defined only in the EF Core model and stored in the database.
    - Use shadow state properties for tracking additional metadata.
    - Configure shadow state properties using the Fluent API.
    - Shadow state properties are useful when you don't want to modify the entity class.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>().Property<DateTime>("CreatedDate");
      ```

42. How do you configure default values in EF Core?
    - Default values are set for properties when a new entity is created.
    - Use the HasDefaultValue or HasDefaultValueSql methods in the Fluent API.
    - Default values can be specified directly or using SQL expressions.
    - They ensure that properties have valid initial values.
    - Default values are useful for setting timestamps, status flags, etc.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>()
          .Property(p => p.CreatedDate)
          .HasDefaultValueSql("GETDATE()");
      ```

43. How do you handle soft deletes in EF Core?
    - Soft deletes mark entities as deleted without physically removing them.
    - Use a boolean property (e.g., IsDeleted) to indicate deletion.
    - Configure a global query filter to exclude soft-deleted entities.
    - Implement logic to set the IsDeleted flag instead of deleting entities.
    - Soft deletes preserve data and support audit logging.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>().HasQueryFilter(p => !p.IsDeleted);
      ```

44. How do you handle complex types (owned entities) in EF Core?
    - Complex types are types that do not have a primary key and are owned by another entity.
    - Use the OwnsOne or OwnsMany methods in the Fluent API to configure owned entities.
    - Owned entities are treated as part of the owner entity.
    - They do not have separate tables and share the same table as the owner.
    - Complex types are useful for value objects and embedded properties.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Order>()
          .OwnsOne(o => o.ShippingAddress);
      ```

45. What is the difference between Find and SingleOrDefault methods in EF Core?
    - Find retrieves an entity by its primary key from the context or database.
    - It returns null if the entity is not found.
    - SingleOrDefault retrieves an entity that matches a given predicate.
    - It throws an exception if more than one entity is found.
    - Use Find for primary key lookups and SingleOrDefault for complex queries.
    - **Example:**
      ```csharp
      var product = context.Products.Find(1); // Find by primary key
      var singleProduct = context.Products.SingleOrDefault(p => p.Name == "Product"); // Query by predicate
      ```

46. How do you handle table splitting in EF Core?
    - Table splitting maps multiple entity types to a single database table.
    - Use the HasOne and HasForeignKey methods to configure table splitting.
    - Specify the table name and foreign key properties.
    - Table splitting is useful for related data that shares the same table.
    - Configure the entities in the OnModelCreating method.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Order>()
          .HasOne(o => o.OrderDetail)
          .WithOne(d => d.Order)
          .HasForeignKey<OrderDetail>(d => d.OrderId);
      modelBuilder.Entity<Order>().ToTable("Orders");
      modelBuilder.Entity<OrderDetail>().ToTable("Orders");
      ```

47. What is the purpose of the HasIndex method in EF Core?
    - HasIndex configures indexes on entity properties to improve query performance.
    - Use it to specify single or composite indexes.
    - Indexes can be unique or non-unique.
    - They help speed up data retrieval operations.
    - Configure indexes in the OnModelCreating method.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>()
          .HasIndex(p => p.Name)
          .IsUnique();
      ```

48. How do you configure cascade delete in EF Core?
    - Cascade delete automatically deletes related entities when the principal entity is deleted.
    - Use the OnDelete method to configure cascade delete behavior.
    - Specify DeleteBehavior.Cascade to enable cascade delete.
    - Cascade delete ensures referential integrity by removing orphaned records.
    - Configure it in the OnModelCreating method.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Category>()
          .HasMany(c => c.Products)
          .WithOne(p => p.Category)
          .OnDelete(DeleteBehavior.Cascade);
      ```

49. How do you handle connection pooling in EF Core?
    - Connection pooling reuses database connections to improve performance.
    - EF Core uses the underlying ADO.NET provider's connection pooling.
    - Configure connection pooling settings in the connection string.
    - Connection pooling reduces the overhead of creating and closing connections.
    - It improves the scalability of applications.
    - **Example:**
      ```csharp
      optionsBuilder.UseSqlServer("your_connection_string;Max Pool Size=100;");
      ```

50. What is the purpose of DbFunctions in EF Core?
    - DbFunctions provide a way to call database functions in LINQ queries.
    - Use the DbFunctions property of DbContext to access database functions.
    - They support built-in and custom database functions.
    - DbFunctions can be used to perform server-side operations.
    - They enhance the expressiveness of LINQ queries.
    - **Example:**
      ```csharp
      var products = context.Products
          .Where(p => EF.Functions.Like(p.Name, "%Product%"))
          .ToList();
      ```

51. How do you configure temporal tables in EF Core?
    - Temporal tables automatically track historical data changes.
    - Configure temporal tables using the ToTable and IsTemporal methods.
    - Specify the history table and period columns.
    - Temporal tables provide built-in history tracking and auditing.
    - Query historical data using the temporal table methods.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>()
          .ToTable("Products", b => b.IsTemporal(
              t => t.HasPeriodStart("PeriodStart").HasPeriodEnd("PeriodEnd")));
      ```

52. How do you handle entity validation in EF Core?
    - Entity validation ensures that data meets certain criteria before saving.
    - Use data annotations like [Required], [MaxLength], and [Range] for validation.
    - Implement the IValidatableObject interface for custom validation logic.
    - EF Core performs validation before saving changes to the database.
    - Handle validation errors using try-catch blocks and display appropriate messages.
    - **Example:**
      ```csharp
      public class Product : IValidatableObject
      {
          public int Id { get; set; }
          [Required]
          public string Name { get; set; }

          public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
          {
              if (Name.Length < 3)
              {
                  yield return new ValidationResult("Name must be at least 3 characters long.");
              }
          }
      }
      ```

53. How do you configure spatial data types in EF Core?
    - EF Core supports spatial data types like geography and geometry.
    - Use the NetTopologySuite package to enable spatial data support.
    - Configure spatial properties using the HasColumnType method.
    - Perform spatial operations using the NetTopologySuite functions.
    - Spatial data types are useful for geographic information systems (GIS).
    - **Example:**
      ```csharp
      modelBuilder.Entity<Location>()
          .Property(l => l.Geolocation)
          .HasColumnType("geography");
      ```

54. How do you configure multiple contexts in EF Core?
    - EF Core supports multiple DbContext classes in a single application.
    - Define separate DbContext classes for different sets of entities.
    - Configure each DbContext with its own connection string and options.
    - Use dependency injection to manage multiple DbContext instances.
    - Ensure that each DbContext is properly configured and registered.
    - **Example:**
      ```csharp
      services.AddDbContext<PrimaryContext>(options => options.UseSqlServer("PrimaryConnectionString"));
      services.AddDbContext<SecondaryContext>(options => options.UseSqlServer("SecondaryConnectionString"));
      ```

55. How do you handle enum mapping in EF Core?
    - EF Core supports mapping enums to database columns.
    - Use enums as properties in entity classes.
    - Configure enum properties using the HasConversion method if needed.
    - EF Core automatically maps enums to their underlying integer values.
    - Enums provide a way to represent fixed sets of values.
    - **Example:**
      ```csharp
      public enum ProductStatus { Available, OutOfStock, Discontinued }
      public class Product
      {
          public int Id { get; set; }
          public ProductStatus Status { get; set; }
      }
      ```

56. How do you handle inheritance in EF Core?
    - EF Core supports three types of inheritance: Table per Hierarchy (TPH), Table per Type (TPT), and Table per Concrete Class (TPC).
    - Configure inheritance using the Fluent API and the HasBaseType method.
    - TPH maps all types in the hierarchy to a single table.
    - TPT maps each type to its own table with shared columns.
    - TPC maps each concrete type to its own table with no shared columns.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Person>().HasDiscriminator<string>("Dis

criminator")
          .HasValue<Employee>("Employee")
          .HasValue<Customer>("Customer");
      ```

57. What is the purpose of ConcurrencyToken in EF Core?
    - Concurrency tokens are used to detect and handle concurrent updates.
    - Use the IsConcurrencyToken method to configure a property as a concurrency token.
    - Concurrency tokens ensure that only one user can update an entity at a time.
    - Handle concurrency conflicts using try-catch blocks and resolve them appropriately.
    - Concurrency tokens help maintain data consistency and integrity.
    - **Example:**
      ```csharp
      modelBuilder.Entity<Product>()
          .Property(p => p.RowVersion)
          .IsRowVersion()
          .IsConcurrencyToken();
      ```

58. How do you handle lazy loading in EF Core?
    - Lazy loading loads related entities on demand when they are accessed.
    - Enable lazy loading by installing the Microsoft.EntityFrameworkCore.Proxies package.
    - Configure lazy loading in the DbContext options.
    - Use virtual navigation properties for lazy loading to work.
    - Lazy loading reduces the initial load time by loading related data as needed.
    - **Example:**
      ```csharp
      services.AddDbContext<ApplicationDbContext>(options =>
          options.UseLazyLoadingProxies().UseSqlServer("your_connection_string"));
      ```

59. How do you handle eager loading in EF Core?
    - Eager loading loads related entities as part of the initial query.
    - Use the Include and ThenInclude methods to specify related entities to load.
    - Eager loading ensures that all necessary data is loaded in a single query.
    - It is useful for scenarios where you need all related data upfront.
    - Eager loading helps avoid the N+1 query problem.
    - **Example:**
      ```csharp
      var orders = context.Orders.Include(o => o.OrderItems).ThenInclude(oi => oi.Product).ToList();
      ```

60. How do you handle explicit loading in EF Core?
    - Explicit loading loads related entities explicitly on demand.
    - Use the Load method to load related entities after the initial query.
    - Explicit loading provides fine-grained control over when related data is loaded.
    - It is useful for optimizing performance and reducing data load.
    - Explicit loading helps avoid loading unnecessary data.
    - **Example:**
      ```csharp
      var order = context.Orders.Find(1);
      context.Entry(order).Collection(o => o.OrderItems).Load();
      ```
