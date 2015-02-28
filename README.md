# drupal6_customize_search
this is drupal6 customize core search module

## Requirements

1. remove 'node' keywords

2. change 'search' for non-English pages

eg: 

<code>
/recherche
/recherche/keyword1
</code>

## update search module

##### /modules/search/search.module

In `search_menu()`, replace
<code>
$items['search'] = array(
</code>
by
<code>
$items[t('search')] = array(
</code>

In `search_get_keys`, replace

<code>
  $path = explode('/', $_GET['q'], 3);
</code>

by
<code>
$path = explode('/', $_GET['q'], 2);
</code>

and
<code>
$return = count($path) == 3 ? $path[2] : $keys;
</code>
by
<code>
$return = count($path) == 2 ? $path[1] : $keys;
</code>

In `search_box_form_submit($form, &$form_state)`, replace
<code>
$form_state['redirect'] = 'search/node/'. trim($form_state['values'][$form_id]);
</code>
by
<code>
$form_state['redirect'] = t('search') . '/'. trim($form_state['values'][$form_id]);
</code>

##### /modules/search/search.pages.inc

Replace
<code>
function search_view($type = 'node') {
</code>
by
<code>
function search_view() {
$type = 'node';
</code>
Replace
<code>
$form_state['redirect'] = 'search/'. $type .'/'. $keys;
</code>
by
<code>
$form_state['redirect'] = t('search') . '/'. $keys;
</code>

## Add translations for "search"

1. in a browser, load the search page for every language. Examples: 'fr/search', 'de/search'.
2. add translations for the string “search” in the admin (/admin/build/translate/search)

## Optional: search page theming

`/search` is themed with `page-search.tpl.php` so its French counterpart, `/recherche`, would be themed with `page-recherche.tpl.php`. To avoid this and keep using `page-search.tpl.php` for all languages, add to `template.php`:
<code>
function phptemplate_preprocess_page(&$vars) {
	if(strpos($vars['body_classes'], 'page-' . t('search'))) {
		$vars['template_files'][] = 'page-search';
	}
}
</code>