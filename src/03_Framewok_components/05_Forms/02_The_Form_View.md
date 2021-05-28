# The Form View

The form view object is the object responsible for computing the html markup of a FormInterface instance.

The default from view embarks a default html structure, but you can create your own if you want by creating your own object, and replacing it in place of the current one in the container at the `'wwp.forms.formView'` key.

Given the following form :

```

$form = $container['wwp.forms.form'];

//Add your first Field
$f1 = new InputField('login','',['label'=>__('username',TEXTDOMAIN)],[Validator::notEmpty()]);
$form->addField($f1);

//Second Field
$f2 = new PasswordField('pwd','',['label'=>__('password',TEXTDOMAIN)],[Validator::notEmpty()]);
$form->addField($f2);

```

The form view could be called like this :

```
//Your form is ready to be rendered.
//This can be done like so : 
$renderOpts  => [
	'formStart' => [
		'action' => '/my/form/action/url',
		'method => 'post'
	],
	'formEnd' => [
		'submitLabel' => 'Log-in'
	]
];
echo $form->getView()->render($renderOpts);
```

And should output : 

```
<form method="post" enctype="multipart/form-data" class="wwpform " action="/my/form/action/url">
	<div class="form-group input-wrap login-wrap">
		<label for="login">Username <span class="required">*</span></label>
		<input type="text" required="required" name="login" id="login" class="form-control text" value="">
	</div>
	<div class="form-group input-wrap pwd-wrap">
		<label for="pwd">Password<span class="required">*</span></label>
		<input type="password" required="required" name="pwd" id="pwd" class="text form-control password" value="">
	</div>
	<div class="submitFormField">
   		<button type="submit" class="btn button">Log-in</button>
	</div>
</form>
```


## Options

As you can see in the example, it's possible to pass some options to the render method. Here's the list :

- formStart // All the html attributes to be place on the form html opening tag.
	- There's one reserved name though : showFormTag, which is a boolean you can pass to not render the < form > tag. (if you have a form object within a form object for instance)
- formEnd : Options relevant to the bottom of the form
	- 'showFormTag'   => true, //whether to show the closing html form tag or not
   - 'showSubmit'    => true, //whether you want a submit btn
   - 'submitLabel'   => __('submit'), //the submit label
   - 'showReset'     => false, //whether you want a reset btn or not
   - 'resetLabel'    => __('reset'), // If yes its label
   - 'btnAttributes' => [ //Html submit attributes
        'type'  => 'submit',
        'class' => 'btn button',
    ],
   - 'resetbtnAttributes' => [ //Html reset attributes
        'type'  => 'reset',
        'class' => 'btn button btn-secondary',
    ], 
