---
layout: post
title: 'Entity Framework 6 error: "Invalid column name ''_ID1'''
categories: [Programming]
tags: [Programming]
date: 2020-02-16 12:30:00 PM UTC
---

<!-- Feb 16, 2020 from abt 06:30:00 PM to 08:30:00 PM Philippine Time -->

There are **two ways** to encounter this error:

### 1. By not following Entity Framework's convention when creating a Navigation Property for your Foreign Key Properties

Let's say you have this model of people and their friendships:

``` csharp
public class Person
{
   public Guid Id { get; protected set; }
   public string Name { get; protected set; }
}
```

<!--more-->


``` csharp
public class Friendship
{
   public Guid Person1Id { get; protected set; }
   public Guid Person2Id { get; protected set; }
}
```

`Person1Id` and `Person2Id` are Foreign Key Properties.

Now, you want to add a Navigation Property for `Person1Id`. You can choose to follow the Code First Conventions of Entity Framework 6, and add a Navigation Propery named `Person1`, like in this code:

``` csharp
public class Friendship
{
   public Guid Person1Id { get; protected set; }
   public Guid Person2Id { get; protected set; }

   public virtual Person Person1 { get; protected set; }
}
```

Or you can choose to name your Navigation Property as `PersonOne`... which will result in the error: ***"Invalid column name 'PersonOne_Id'."***

The **solution** is to add this in your configuration: `.HasRequired(x => x.PersonOne).WithMany().HasForeignKey(x => x.Person1Id)`

``` csharp
public class PeopleDbContext : DbContext
{
   protected override void OnModelCreating(DbModelBuilder modelBuilder)
   {
      ...
   
      modelBuilder.Entity<Friendship>()
         .ToTable("Friendship")
         .HasKey(x => new { x.Person1Id, x.Person2Id })
         .HasRequired(x => x.PersonOne)
            .WithMany()
            .HasForeignKey(x => x.Person1Id);        
   }
}
```

### 2. By adding a Collection Navigation Property in your _principal_ entity without proper configuration.

Still using our example above, the `Person` entity is called a _principal_ entity, while the `Friendship` entity is called a _dependent_ entity.

Now, you want to add a Collection Navigation Property in your _principal_ entity, `Person`. This Collection Navigation Property will hold the list of friends of a person.

``` csharp
public class Person
{
   public Guid Id { get; protected set; }

   public virtual ICollection<Friendship> Friendships { get; protected set; }
}
```

This will result in an error: ***"Invalid column name 'Person_Id'."***

The **solution** is to add `.WithMany(x => x.Friendships)` in your configuration.

``` csharp
public class PeopleDbContext : DbContext
{
   protected override void OnModelCreating(DbModelBuilder modelBuilder)
   {
      ...
   
      modelBuilder.Entity<Friendship>()
         .ToTable("Friendship")
         .HasKey(x => new { x.Person1Id, x.Person2Id })
         .HasRequired(x => x.PersonOne)
            .WithMany(x => x.Friendships)
            .HasForeignKey(x => x.Person1Id);        
   }
}
```

----------


If you want to have a first-hand experience of the error, you can download and ***play with*** the code from github: [https://github.com/jeremiahflaga/entity-framework-6-playground](https://github.com/jeremiahflaga/entity-framework-6-playground). After downloading go to the folder `\entity-framework-6-playground\EF6Playground.Tests\2020-02-09-invalid-column-name-_ID` and run the test inside the `PeopleDbContextTests.cs` file...


----------

**Sources:**

- answer of [@Proukuros](https://stackoverflow.com/users/1721298/prokurors) to a similar question in StackOverflow: [https://stackoverflow.com/a/53136366](https://stackoverflow.com/a/53136366)
- [https://www.learnentityframeworkcore.com/relationships](https://www.learnentityframeworkcore.com/relationships) for the terms _principal entity_ and _dependent entity_
