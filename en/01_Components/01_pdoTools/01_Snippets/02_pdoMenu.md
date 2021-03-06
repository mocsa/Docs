Snippet generation menu. It can replace Wayfinder, and allows more flexibility to specify properties.

For example, it can build menus from several parents, showing them both together and as separate branches.

Provides a substantial increase in speed, although only on first page view if the Wayfinder menu is cached. 

## Properties

Title				| Default		| Description
------------------------|-------------------|------------------------------------------------------------------------------------------
**&parents**			| Current resource	| Comma-separated list of parents to search for results. If set to 0 will use all top-level resources. If the parent ID starts with a hyphen, it and its children are excluded from the result.
**&level**				| 0 (no limit)| The level of the generated menu.
**&resources**			|  					| Comma-separated list of resources to add into the results. If the ID of the resource starts with a dash, this resource is excluded from the results.
**&templates**			|  					| Comma-separated list of templates to filter results. If the template ID starts with a dash, resources using it are excluded from the results. 
**&where**				|  					| An array of additional parameters encoded as JSON.
**&displayStart**		| 0					| Include display of parent node. It is useful when you specify more than one «parents».
**&context**			|  					| Specifying the Context of the resources to build the menu from.
**&showHidden**			| 0					| Show resources that are marked to be hidden in the menu.
**&showUnpublished**	| 0					| Show unpublished resources to everyone.
**&previewUnpublished**	| 0					| Show unpublished resources only for logged-in Manager users with permission to view unpublished.
**&hideSubMenus**		| 0					| Hide inactive submenu branches.
**&select**				|  					| Comma-separated list of fields to retrieve. You can specify a JSON string array , for example {"modResource":"id,pagetitle,content"}.
**&sortby**				| menuindex			| Any resource field for sorting, including TV option the property **includeTVs** is set. A JSON string can be specified with an array of several fields. To randomly select sorting use «RAND()».
**&sortdir**			| ASC				| Sort Direction: Ascending or Descending.
**&limit**				| 0					| Limit the number of results. Can be set to «0» for no limit.
**&offset**				| 0					| Number of items to skip from the beginning.
**&checkPermissions**	|  					| Specify which user permissions to check when listing resources. If no permissions are specified, permissions will not be checked at all.
**&countChildren**		| 0					| Count of the number of children of each parent resource and output it to the placeholder `[[+children]]`. It makes additional queries to the database, so is 0 by default.
**&toPlaceholder**	|  							| If not empty, the snippet will save its output to a placeholder with the same name, instead of returning the generated menu.
**&plPrefix**		| wf.						| Prefix for placeholders used in the template chunks
**&showLog**		| 0							| Show debugging details on the processing of the snippet. Only displayed to logged-in Manager users.
**&fastMode**		| 0							| Quick mode for processing chunks. All raw tags (output modifiers, snippets, etc.) are removed.
**&cache**			| 0							| Caching snippet results.
**&cacheTime**		| 3600						| Duration of the cache in seconds.
**&scheme**			| -1 (relative to site_url)| How the URL is generated, based on values valid for the modX::makeUrl() API.
**&useWeblinkUrl**	|  							| When set to 1, the URL specified in a weblink resource will be output to the placeholder «[[+link]]» instead of the link to the weblink resource itself. 
**&rowIdPrefix**	|  							|  If set, this parameter creates a unique ID for each item. The value will be rowIdPrefix + docId. 
**&hereId**			|  							| Define the current ID to use for the snippet. Use a value of [[*id]] if the template specified by hereTpl and activeRowParentTpl is not applied correctly to the menu item. 
**&includeTVs**			|  							| Define comma delimited list of TVs to include. 
**&tvPrefix**			|  							| Define tvPrefix eg. tv.

### Template Properties
These properties specify the chunks that contain the templates to format the parts of the generated menus.

Title				| Description
------------------------|------------------------------------------------------------------------------------------
**&tplOuter**			| Chunk to wrap the entire menu block. default: `@INLINE <ul[[+classes]]>[[+wrapper]]</ul>`
**&tpl**				| Chunk for processing each resource. If not specified, the contents of the resource fields will be printed to the screen. Default: `@INLINE <li[[+classes]]><a href="[[+link]]" [[+attributes]]>[[+menutitle]]</a>[[+wrapper]]</li>`
**&tplParentRow**		| Chunk for container item with children.
**&tplParentRowHere**	| Chunk for current container item.
**&tplHere**			| Chunk for the current resource
**&tplInner**			| Chunk wrapper for submenu sections. If empty will use **&tplOuter**
**&tplInnerRow**		| Chunk for submenu rows.
**&tplInnerHere**		| Chunk for submenu current item.
**&tplParentRowActive**	| Chunk for active category.
**&tplCategoryFolder**	| Special chunk for category resources. Category resources have isfolder = 1, and «(empty)» template or attribute «rel = category». Useful for creating top-level menu items that are not themselves active links.
**&tplStart**			| Chunk for the parent resource, provided that **&displayStart** is also used. Default: `@INLINE <h2[[+classes]]>[[+menutitle]]</h2>[[+wrapper]]`

### Properties for CSS classes
These properties specify the value of the placeholder «[[+classes]]» and «[[+classnames]]» for the various parts of the menu.

Title			| Description
--------------------|-------------------------------------------------------
**&firstClass**		| Class for the first menu item. Default: **first**
**&lastClass**		| Class for the last menu item. Default: **last**
**&hereClass**		| Class for the active menu item and its parents. Default: **active**
**&parentClass**	| Class for parent items.
**&rowClass**		| Class for each menu row.
**&outerClass**		| Class for outer wrapper.
**&innerClass**		| Class for inner submenu wrappers.
**&levelClass**		| Class for each level of the menu. For example, if you specify «level», it will produce «level1», «level2» etc.
**&selfClass**		| Class for the current page in the menu.
**&webLinkClass**	| Class for weblink resources.


## Examples
A simple top-level menu:

```
[[pdoMenu?
	&parents=`0`
	&level=`1`
]]
```

2-level menu with the exception of certain parents:

```
[[pdoMenu?
	&parents=`-10,-15`
	&level=`2`
	&checkPermissions=`load,list,view`
]]
```

Menu generated from two parents, showing the parents:

```
[[pdoMenu?
	&parents=`10,15`
	&displayStart=`1`
]]
```

List all resources in one step:

```
[[pdoMenu?
	&parents=`0`
	&level=`2`
	&tplInner=`@INLINE [[+wrapper]]`
	&tplParentRow=`@INLINE <li[[+classes]]><a href="[[+link]]" [[+attributes]]>[[+menutitle]]</a> ([[+children]])</li>[[+wrapper]]`
	&countChildren=`1`
]]
```


[0]: http://rtfm.modx.com/revolution/2.x/developing-in-modx/other-development-resources/class-reference/modx/modx.makeurl
