# The Assets component

A WonderWp package allowing you to work with assets. It helps you declare them, enqueue them, and work with external systems such as builders (webpack, gulp...) if needed.

## Declaring assets

- Declaring assets works with Assets Services. Documentation about Assets Services can be found [here](../../02_Creating_a_plugin/04_Services/03_Assets_service.md).

## Enqueuing assets

- Using your assets within your theme works with Assets Enqueuers.

### Declaring an enqueueur

By default, wonderwp works with the `DirectAssetEnqueuer`, which is basically a gateway of synthetic sugar or traditionnal WordPress methods.

You have two other enqueuers available if you want : the `JsonAssetEnqueuer` and the `PackageAssetEnqueuer`, which are more meant to be used with external tooling such as gulp or webpack.

Documentation coming soon.
