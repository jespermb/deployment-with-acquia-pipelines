## Deploy with acquia pipelines example.

Add this if when deploying a javascript component build during deployment.

```
/**
 * Implements hook_library_info_build().
 */
function example_preact_library_info_build() {
  $libraries = [];

  $libraries['preactapp'] = [
    'dependencies' => [
      'core/drupal',
      'core/drupalSettings',
    ],
  ];

  $module_handler = \Drupal::service('module_handler');
  $module_path = $module_handler->getModule('example_preact')->getPath();
  $component_path = 'components/example_preact_app';
  $manifest_path = $module_path . '/' . $component_path . '/build/push-manifest.json';
  $manifest = Json::decode(file_get_contents($manifest_path));
  foreach ($manifest['/'] as $file_name => $info) {
    if ($info['type'] === 'script') {
      $libraries['preactapp']['js'][$component_path . '/build/' . $file_name] = [];
    }
  }

  return $libraries;
}
```