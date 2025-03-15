# WonderWp Utility Functions

WonderWp defines a few utility functions that are used by the framework and that you can use too.

- `array_merge_recursive_distinct` : slightly different from array_merge as it does not combine results
- `implode_recursive` : implode, but recursive
- `redirect` : Redirect to a url either via the back end if headers have not been sent, or via the frontend otherwise
- `paramsToHtml` : Takes an array of params and builds html arguments with them
- `get_plugin_file` :  Locates and returns plugin file path, either in the theme if the override version exists, or in the plugin directory otherwise
- `include_plugin_file` : Includes file found with previous function
- `isAjax` : Test if a request has been made via ajax or not
