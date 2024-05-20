# Migration Vs Factory Vs Seed Vs Model

1. Migration: defines the database schema of the model , rows , columns and datatypes, creates and drops the table using functions up and down

2. Factory: defines the data to be stored inside the database table. It is only a template, does not run by itself

3. Seeder: this runs the factory based on the template. Can include rules here to define how many instances to create and constraints

4. Model: define the relationships between each entity here and is the entity itself. If there is dependency with another model, need to add another function here based on the field name, for example createdBy
