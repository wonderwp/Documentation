# Form Fields definitions

In this section you'll retrieve more information about each object implementing the `FieldInterface` interface. Which means you can add them to your forms.

Every pre defined Field Object inherits from the `WonderWp\Component\Form\Field\AbstratField` class.

From the code of this class you can see that the AbstractFields take 4 parameters in their constructor:

- A **name**, the input name
- A **value**, the input value
- Some **display rules** : an array of rules defining how the field would be displayed
- Some **validation rules** : an array of rules defining how the field would be validated

Those 4 parameters are valid for every Abstract Field inheritance.

## Display Rules 

### Reserved Rules

This array have 5 reserved keys :

- label : The label text
- labelAttributes : html label attributes (ex id, for, class...)
- help : help text
- wrapAttributes : the input html wrap attributes (ex id, class, data-*, ...)
- inputAttributes : the input html attributes (ex id, class, ...)

## Validation Rules

WonderWp uses [Respect/Validation](http://respect.github.io/Validation/) as a validator. All the validation rules come from this library. You can find the list of valdiation rules on this page : [http://respect.github.io/Validation/docs/validators.html](http://respect.github.io/Validation/docs/validators.html)

## Text Input Fields

Here you can find the list of all text like inputs and how to instanciate them :

### Text Input

```
use WonderWp\Component\Form\Field\InputField;
$field = new InputField('my-input-field-name','field-value', ['label'=>'Test Text Field'],[Validator::notEmpty()]);
```

### Date Input

```
use WonderWp\Component\Form\Field\DateField;
$field = new DateField('my-date-field-name','2018-05-25', ['label'=>'Test Date Field'],[Validator::notEmpty()]);
```

### Email Input

```
use WonderWp\Component\Form\Field\EmailField;
$field = new EmailField('my-email-field-name','test@test.com', ['label'=>'Test Email Field'],[Validator::notEmpty()]);
```

### File Input

```
use WonderWp\Component\Form\Field\FileField;
$field = new FileField('my-file-field-name','/path/to/file', ['label'=>'Test File Field'],[Validator::notEmpty()]);
```

### Hidden Input

```
use WonderWp\Component\Form\Field\HiddenField;
$field = new HiddenField('my-hidden-field-name','field-value');
```

### Nonce field

WordPress nonce field.

```
use WonderWp\Component\Form\Field\NonceField;
$field = new NonceField('nonceName');
```

### Numeric Input

```
use WonderWp\Component\Form\Field\NumericField;
$field = new NumericField('my-numeric-field-name',10, ['label'=>'Test Numeric Field'],[Validator::notEmpty()]);
```

### Password Input

```
use WonderWp\Component\Form\Field\PasswordField;
$field = new PasswordField('my-password-field-name']);
```

### Url Input

```
use WonderWp\Component\Form\Field\UrlField;
$field = new UrlField('my-url-field-name','https://wonderwp.net');
```

## Checkboxes Fields

### Checkbox Input

```
use WonderWp\Component\Form\Field\CheckBoxField;
$field = new CheckBoxField('my-checkbox-field-name', 1);
```

### Checkboxes group

As it can be fastidious to generate a group of checkboxes, there's a special class to do just that : 

```
$opts = [
	'val1' => 'Label 1',
	'val2' => 'Label 2',
	'val3' => 'Label 3',
	'val4' => 'Label 3',			
];
$selectedCategories = [
	'val3'
];
$cbGroup = new CheckBoxesField('my-checkboxes-group', $selectedCategories);
$cbGroup->setOptions($opts)->generateCheckBoxes();
```

This will output :

- [  ] Label 1
- [  ] Label 2
- [x] Label 3
- [  ] Label 4

## Radio Fields

### Radio Input

```
$opts = [
	'val1' => 'Label 1',
	'val2' => 'Label 2',
	'val3' => 'Label 3',
	'val4' => 'Label 3',			
];
$selectedCategories = [
	'val3'
];
$group = new RadioField('my-radio-field', $selectedCategories);
$group->setOptions($opts)->generateRadios();
```

## Textareas

### Textarea

```
use WonderWp\Component\Form\Field\TetAreaField;
$field = new TextAreaField('my-textarea-field-name','text-value', ['label'=>'Test Textarea'],[Validator::notEmpty()]);
```

### wp Editor

If a textarea is not enough and you're looking for a more capable area, you can generate an editor field instead in the admin :

```
use WonderWp\Component\Form\Field\EditorField;
$field = new EditorField('my-editor-field-name','text-value', ['label'=>'Test wp Editor'],[Validator::notEmpty()]);
```

## Selects

### Simple select


```
use WonderWp\Component\Form\Field\SelectField;
$opts = [
	'val1' => 'Label 1',
	'val2' => 'Label 2',
	'val3' => 'Label 3',
	'val4' => 'Label 3',			
];
$select = new SelectField('my-radio-field', 'val3');
$select->setOptions($opts);
```

### Multiple select

```
use WonderWp\Component\Form\Field\SelectField;
$opts = [
	'val1' => 'Label 1',
	'val2' => 'Label 2',
	'val3' => 'Label 3',
	'val4' => 'Label 3',			
];
$select = new SelectField('my-radio-field', 'val3',['inputAttributes'=>['multiple'=>true]]);
$select->setOptions($opts);
```
