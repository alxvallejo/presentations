Assign a custom theme wrapper on hook_view_alter

<aside class="notes">
    Only in hook_view_alter can you access the menu block's level and other attributes
</aside>

```php
function wellpet_trufood_block_view_alter(&$data, $block) {
    // Assign a new theme wrapper for this menu block's level
    if ($block->delta == '3') {
        $menu_name = $data['content']['#config']['menu_name'];
        $level = $data['content']['#config']['level'];
        if ($menu_name == 'main-menu' && $level == '1') {
            $data['content']['#content']['#theme_wrappers']
                = array('menu_tree__menu_block__3__level1');
        }
    }
}

function wellpet_trufood_menu_tree__menu_block__3__level1($vars) {
    // Move Home link to top of sitemap
    if ($_GET['q'] == 'sitemap') {
        $vars['tree'] = '<li class="menu-level-1">
            <a href="' . url('<front>') . '">Home</a>
            </li>' . $vars['tree'];
    }
    return $vars['tree'];
}
```