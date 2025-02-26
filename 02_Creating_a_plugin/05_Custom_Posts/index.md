# Custom Post

## Generating a custom post type plugin

The generator now supports CPT plugin generation.

You'll find the command line to execute in
the [generator documentation page](../02_Generator.md#page_Generating_a_Custom_Post_Type_plugin).

## Extra files generated for a CPT plugin

In case of a CPT plugin, the generator will generate some extra files and folders compared to the basic plugin
architecture :

- **CPT/**
    - [...]CPT.php : The [CPT Definition file](./The_CPT_Definition_file.md) _//CityCpt.php if your plugin is called
      City_
- **Repository/**
    - [...]Repository.php : [The CPT Repository](./The_CPT_repository.md) _//CityRepository.php if your plugin is called
      City_
    
