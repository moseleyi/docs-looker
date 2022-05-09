# Views

### Single reference to the table column per field

In a specific view, there should only be one reference to the table column i.e., `${TABLE}.name`, and any other references should be done using LookML referencing `${name}`. This allows us to have only one place where we need to make a change in case a column changes its name in the Data Model.

