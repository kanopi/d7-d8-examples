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

### Using settings

### Drupal 7

Located within `example.module` file.

```php
/**
 * Implements hook_block_info().
 */
function example_block_info() {
    $blocks = [];
    $blocks['test_config_block'] = [
        'info' => t('Test Config ')
    ];
    return $blocks;
}

/**
 * Implements hook_block_configure().
 */
function example_block_configure($delta = '') {
  $form = array();
  if ($delta == 'test_config_block') {
    $form['block_example_string'] = array(
      '#type' => 'textfield',
      '#title' => t('Block contents'),
      '#size' => 60,
      '#description' => t('This text will appear in the example block.'),
      '#default_value' => variable_get('block_example_string_text', t('A default value.')),
    );
  }
  return $form;
}

/**
 * Implements hook_block_save().
 */
function example_block_save($delta = '', $edit = array()) {
  if ($delta == 'test_config_block') {
    variable_set('block_example_string_text', $edit['block_example_string']);
  }
}

/**
 * Implements hook_block_view().
 */
function example_block_view($delta = '') {
    $block = [];
    if ($delta == 'test_config_block') {
        $block['subject'] = t('Test Config Block');
        $block['content'] = variable_get('block_example_string_text', t('A default value.'));
    }
    return $block;
}
```

### Drupal 8

Located within `example/src/Plugin/Block/TestConfigBlock.php` file

```php
<?php

namespace Drupal\example\Plugin\Block;

use Drupal\Core\Block\BlockBase;
use Drupal\Core\Form\FormStateInterface;

/**
 * Provides a 'Test Config Block' block.
 *
 * @Block(
 *   id = "test_config_block",
 *   admin_label = @Translation("Test Config Block")
 * )
 */
class TestConfigBlock extends BlockBase {

  /**
   * {@inheritdoc}
   *
   * This method sets the block default configuration. This configuration
   * determines the block's behavior when a block is initially placed in a
   * region. Default values for the block configuration form should be added to
   * the configuration array. System default configurations are assembled in
   * BlockBase::__construct() e.g. cache setting and block title visibility.
   *
   * @see \Drupal\block\BlockBase::__construct()
   */
  public function defaultConfiguration() {
    return [
      'block_example_string' => $this->t('A default value. This block was created at %time', ['%time' => date('c')]),
    ];
  }

  /**
   * {@inheritdoc}
   *
   * This method defines form elements for custom block configuration. Standard
   * block configuration fields are added by BlockBase::buildConfigurationForm()
   * (block title and title visibility) and BlockFormController::form() (block
   * visibility settings).
   *
   * @see \Drupal\block\BlockBase::buildConfigurationForm()
   * @see \Drupal\block\BlockFormController::form()
   */
  public function blockForm($form, FormStateInterface $form_state) {
    $form['block_example_string_text'] = [
      '#type' => 'textarea',
      '#title' => $this->t('Block contents'),
      '#description' => $this->t('This text will appear in the example block.'),
      '#default_value' => $this->configuration['block_example_string'],
    ];
    return $form;
  }

  /**
   * {@inheritdoc}
   *
   * This method processes the blockForm() form fields when the block
   * configuration form is submitted.
   *
   * The blockValidate() method can be used to validate the form submission.
   */
  public function blockSubmit($form, FormStateInterface $form_state) {
    $this->configuration['block_example_string'] = $form_state->getValue('block_example_string_text');
  }

  /**
   * {@inheritdoc}
   */
  public function build() {
    return [
      '#markup' => $this->configuration['block_example_string'],
    ];
  }
}
```

## Drupal CLI Command

```
drupal generate:plugin:block [options]
```

[Read more about command](https://hechoendrupal.gitbooks.io/drupal-console/content/en/commands/generate-plugin-block.html)
