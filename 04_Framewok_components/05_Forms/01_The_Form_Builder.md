# The Form Builder

The form builder came from the need to allow developers to manipulate form objects instead of form markup, in order to create forms programmatically. 

## How to create a form

1. WonderWp provides a form factory available in the container. Ask it to give you a new instance like so : `$form = $container['wwp.form.form'];`
2. Once you've got your instance, add some fields to it via its `addField` method. You can find the complete list of available fields objects under WonderWp/Component/Form/Field namespace. The next document section describes them all if you want to know more about the parameters they use.

## Example

```

/** @var FormInterface $form */
$form = $container['wwp.form.form']; //Get a new form instance from the container factory

$form->setName('er-login-form'); //You can set a name if you want

//Add your first Field
$f1 = new InputField('login',$loginVal,['label'=>__('username',TEXTDOMAIN)]);
$form->addField($f1);

//Second Field
$f2 = new PasswordField('pwd',$pwdVal,['label'=>__('password',TEXTDOMAIN)]);
$form->addField($f2);

//Your form is ready to be rendered.
//This can be done like so : 
echo $form->getView()->render();

```

In the next doc section, let's see the different fields you can manipulate and how you can use them.

