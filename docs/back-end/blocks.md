# Building Blocks

## Basic Blocks

### Drupal 7

Located within `example.module` file.

```php
/**
 * Implements hook_block_info().
 */
function example_block_info() {
    $blocks = [];
    $blocks['test'] = [
        'info' => t('Test Block')
    ];
    $blocks['test_list'] = [
        'info' => t('Test List')
    ];
    return $blocks;
}

/**
 * Implements hook_block_view().
 */
function example_block_view($delta = '') {
    $block = [];

    if ($delta == 'test') {
        $block['subject'] = t('Test Title');
        $block['content'] = t('This is a test block');
    }
    elseif ($delta == 'test_list') {
        $block['subject'] = t('Test List');
        $block['content'] = [
            '#theme' => 'item_list',
            'items' => [
                l('Home', '<front>'),
                l('Admin', '/admin')
            ],
            'title' => t('Test List'),
        ];
    }

    return $block;
}
```

### Drupal 8

Located within `example/src/Plugin/Block/TestBlock.php` file

```php
<?php

namespace Drupal\example\Plugin\Block;

use Drupal\Core\Block\BlockBase;

/**
 * Provides a 'Test' block.
 *
 * @Block(
 *   id = "test",
 *   admin_label = @Translation("Test Block")
 * )
 */
class TestBlock extends BlockBase {

  /**
   * {@inheritdoc}
   */
  public function build() {
    return [
        '#markup' => $this->t('This is a test block'),
    ];
  }

}
```

Located within `example/src/Plugin/Block/TestListBlock.php` file

```php
<?php

namespace Drupal\example\Plugin\Block;

use Drupal\Core\Block\BlockBase;

/**
 * Provides a 'Test List' block.
 *
 * @Block(
 *   id = "test_list",
 *   admin_label = @Translation("Test List")
 * )
 */
class TestList extends BlockBase {

  /**
   * {@inheritdoc}
   */
  public function build() {
    return [
        '#theme' => 'item_list',
        'items' => [
            l('Home', '<front>'),
            l('Admin', '/admin')
        ],
        'title' => t('Test List'),
    ];
  }

}
```

## Advanced Blocks

@todo: Add example of block with form.

## Drupal CLI Command

```
drupal generate:plugin:block [options]
```

[Read more about command](https://hechoendrupal.gitbooks.io/drupal-console/content/en/commands/generate-plugin-block.html)
