# The Form Validator

The form validator is the object that allows you to validate a form instance based on a given set of data.

As you can read in the Form Fields chapter, you can define validation rules with each field you add to a form builder.

The form validator takes a form instance, then a set of data, and analyzes that to tell you whether the form is valid or not, and which fields are not valid.

## Validating a form

```
// Form Validation
$formValidator = $container['wwp.form.validator'];
$formValidator->setFormInstance($myForm); //$myForm being an instance of FormInterface
$errors = $formValidator->validate($data); // Data being an array, (coming from the Request object for instance)
```

If the `$errors` array is empty, then your form is valid, otherwise, you get the details of what went wrong in the array.
