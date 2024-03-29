# The CPT Definition file

This file is where you specify your CPT declaration based on
the [register_post_type()](https://developer.wordpress.org/reference/functions/register_post_type/) function.

Here's an example of a CPT file generated by the generator.

```
<?php

namespace Jdmweb\Plugin\City\CPT;

use WonderWp\Component\CPT\CustomPostType;
use function WonderWp\Functions\array_merge_recursive_distinct;

class CityCPT extends CustomPostType
{
    /**
     * CityCPT CPT constructor.
     *
     * @param string $name
     * @param array  $passed_opts CPT options, @see https://developer.wordpress.org/reference/functions/register_post_type/
     * @param string $taxonomy_name
     * @param array  $passed_taxonomy_opts
     */
    public function __construct($name = '', array $passed_opts = [], $taxonomy_name = '', array $passed_taxonomy_opts = [])
    {
        $name        = WWP_PLUGIN_CITY_NAME;
        $defaultOpts = [
            'labels'       => [
                //CPT labels can be edited below
                'name' => ucfirst(__($name, WWP_CITY_TEXTDOMAIN)),
                //'not_found'    => __($name . ' not found', WWP_CITY_TEXTDOMAIN),
                //'add_new_item' => __('Add new ' . $name, WWP_CITY_TEXTDOMAIN),
                //'edit_item'    => __('Edit ' . $name, WWP_CITY_TEXTDOMAIN),
            ],
            'show_in_rest' => true,
            'rewrite'      => ['slug' => __(sanitize_title($name), WWP_CITY_TEXTDOMAIN)],
        ];
        $opts        = array_merge_recursive_distinct($defaultOpts, $passed_opts);
        parent::__construct($name, $opts, $taxonomy_name, $passed_taxonomy_opts);
    }
}

```

This file is then automatically loaded by your plugin manager to register your CPT with WordPress.

## Custom Fields for your CPT

It's possible to declare metas that will be turned into a post meta form. Metas declared this way are automatically
saved upon post save.

### Basic custom fields declaration

Custom Post Fields can be declared via the $metaDefinitions property.

The syntax is as follows :

```
$this->setMetaDefinitions([
   $fieldName=>[$fieldType, $fieldDefaultValue, array $displayRules, array $validationRules],
   $fieldName=>[$fieldType, $fieldDefaultValue, array $displayRules, array $validationRules],
   //etc 
]);
```

To learn more about the acceptable values for `$fieldType`, `$displayRules` and `$validationRules`, refer to
the [form documentation](../../03_Framewok_components/05_Forms/02_The_Form_Fields.md).

Example of the creation of 6 custom fields for your CPT :

```
class CaseStudyCpt extends CustomPostType
{
    public function __construct($name = '', array $passed_opts = [], $taxonomy_name = '', array $passed_taxonomy_opts = [])
    {
        $name        = WWP_PLUGIN_CASESTUDIES_NAME;
        $defaultOpts = [
            'labels'       => [
                'name' => 'Case Studies',
            ],
            'show_in_rest' => true,
            'rewrite'      => ['slug' => 'case-study'],
        ];
        $opts        = array_merge_recursive_distinct($defaultOpts, $passed_opts);
        parent::__construct($name, $opts, $taxonomy_name, $passed_taxonomy_opts);

        $this->setMetaDefinitions([
            'cs-year'    => [NumericField::class, null, ['label' => 'Year of completion']],
            'cs-color'   => [InputField::class, null, ['label' => 'Ambiant color']],
            'cs-temp-link'   => [UrlField::class, null, ['label' => 'Temporary url']],
            'cs-desktop' => [MediaField::class, null, ['label' => 'Desktop Visual']],
            'cs-tablet' => [MediaField::class, null, ['label' => 'Tablet Visual']],
            'cs-mobile' => [MediaField::class, null, ['label' => 'Mobile Visual']],
        ]);
    }
}
```

#### More advanced custom fields declaration

There will be times when you'll need to create more advanced fields definitions, and the basic way won't be sufficient.

For this, you can specify meta definitions with callables :

```
class CaseStudyCpt extends CustomPostType
{
    public function __construct($name = '', array $passed_opts = [], $taxonomy_name = '', array $passed_taxonomy_opts = [])
    {
        $name        = WWP_PLUGIN_CASESTUDIES_NAME;
        $defaultOpts = [
            'labels'       => [
                'name' => 'Case Studies',
            ],
            'show_in_rest' => true,
            'rewrite'      => ['slug' => 'case-study'],
        ];
        $opts        = array_merge_recursive_distinct($defaultOpts, $passed_opts);
        parent::__construct($name, $opts, $taxonomy_name, $passed_taxonomy_opts);

        $this->setMetaDefinitions([
            'cs-year'        => [$this, 'getCsYear'], //Meta Declaration as callable, which is defined below
            'cs-test-select' => [$this, 'getSelectTest']
        ]);
    }

    //Meta definitions callables :

    public function getCsYear($metaKey, $savedMetaValue)
    {
        //This is equivalent to [NumericField::class, null, ['label' => 'Year of completion']]
        return new NumericField($metaKey,$savedMetaValue,['label' => 'Year of completion']);
    }
    
    //Example of a field that can't be declared with a simple array meta definition,
    //Because for select fields we need to specify options. 
    public function getSelectTest($metaKey, $savedMetaValue)
    {
        $select = new SelectField($metaKey,$savedMetaValue,['label' => 'Year of completion']);
        $select->setOptions([
            ''=>'Choose a value',
            1=>'First Value',
            2=>'Second Value',
        ]);        
        
        return $select;
    }    
```

### Using custom fields in the admin

#### Using php panels

WonderWp can register your panels automatically.

To do so, add these lines to you hook service (usually they are generated for you by the CPT generator) : 

```
//This is your CPT declaration object.
$cpt = $this->manager->getConfig('cpt');

//CustomPostTypeService is the CPT service that interacts with your CPT instance.
//It is by default a WonderWp object.
//If you use your own CPT service, make sure it extends CustomPostTypeService or implements the createMetasForm and saveMetasForm methods.

/** @var CustomPostTypeService $cptService */
$cptService = $this->manager->getService(ServiceInterface::CUSTOM_POST_TYPE_SERVICE_NAME);
if(!empty($cpt->getMetaDefinitions())){
	//This automatically creates panel forms based on your meta definitions
    add_action('admin_init', [$cptService, 'createMetasForm']);
    //This automtically saves the posted values
    add_action('save_post', [$cptService, 'saveMetasForm'], 10, 2);
}
```

#### Using Gutenberg

You can manipulate your post metas via Gutenberg AdminPanels or custom sidebars.

To do so, you metas need to be registered with the Rest API.

WonderWp can register them automatically.

To do so, add these lines to you hook service : 

```
//This is your CPT declaration object.
$myCpt = $this->manager->getConfig('cpt');

//CustomPostTypeService is the CPT service that interacts with your CPT instance.
//It is by default a WonderWp object.
//If you use your own CPT service, make sure it extends CustomPostTypeService or implements the makeMetasAvailableInRestApi method.

/** @var CustomPostTypeService $cptService */
$cptService = $this->manager->getService(ServiceInterface::CUSTOM_POST_TYPE_SERVICE_NAME);
if (!empty($cpt->getMetaDefinitions())) {
    $this->addAction('init', [$cptService, 'makeMetasAvailableInRestApi']);
}
```

## Using multiple slugs for CPT rewrite rules

When declaring a CPT, in WordPress there's
a [rewrite](https://developer.wordpress.org/reference/functions/register_post_type/#rewrite) option attribute :

```
class ActuCPT extends CustomPostType
{
    const localeMeta  = 'news_locale';
    const listImgMeta = 'news_list_img';

    public function __construct($name = '', array $passed_opts = [], $taxonomy_name = '', array $passed_taxonomy_opts = [])
    {
        $name = !empty($name) ? $name : WWP_PLUGIN_ACTU_NAME;

        $defaultOpts = [
            'rewrite'             => ['slug' => 'news'],
        ];
        $opts        = array_merge_recursive_distinct($defaultOpts, $passed_opts);

        parent::__construct($name, $opts, $taxonomy_name, $passed_taxonomy_opts);
    }
```

In WonderWp, you can specify an array for this rewrite value, and custom post pages will be available on all of them.

This is useful for multilingual needs, for example :

```
class ActuCPT extends CustomPostType
{
    const localeMeta  = 'news_locale';
    const listImgMeta = 'news_list_img';

    public function __construct($name = '', array $passed_opts = [], $taxonomy_name = '', array $passed_taxonomy_opts = [])
    {
        $name = !empty($name) ? $name : WWP_PLUGIN_ACTU_NAME;

        $defaultOpts = [
            'rewrite'             => ['slug' => ['news','actualites','anotherslug']],
        ];
        $opts        = array_merge_recursive_distinct($defaultOpts, $passed_opts);

        parent::__construct($name, $opts, $taxonomy_name, $passed_taxonomy_opts);
    }
```