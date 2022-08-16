# Refinements

Override All Hidden Fields

Sometimes the main view may contain mayn fields that are not used in all the explores, which is achiveved by using the following parameter:

```
view: test {
  fields_hidden_by_default: yes
}  
```

However, a refinement is a good way to override this setting, if all fields from refinement file are to be shown. It stops us from having to add `hidden: no` to all the fields in the refinement. Instead, we add:

```
view: +test {
  fields_hidden_by_default: no
}
```
