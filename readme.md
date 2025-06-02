# Nova Attach Many

**This is a fork of [dillingham/nova-attach-many](https://github.com/dillingham/nova-attach-many) with additional features, including support for exclusive selection items.**

**Compatible with Laravel Nova 4 and 5.**


[![Latest Version on Github](https://img.shields.io/github/release/dillingham/nova-attach-many.svg?style=flat-square)](https://packagist.org/packages/dillingham/nova-attach-many)
[![Total Downloads](https://img.shields.io/packagist/dt/dillingham/nova-attach-many.svg?style=flat-square)](https://packagist.org/packages/dillingham/nova-attach-many) [![Twitter Follow](https://img.shields.io/twitter/follow/im_brian_d?color=%231da1f1&label=Twitter&logo=%231da1f1&logoColor=%231da1f1&style=flat-square)](https://twitter.com/im_brian_d)

Belongs To Many create & edit form UI for Nova. Enables attaching relationships easily and includes validation.

![attach-many](https://user-images.githubusercontent.com/29180903/52160651-be7fd580-2687-11e9-9ece-27332b3ce6bf.png)

### Installation

Compatible with Laravel Nova 4 and 5.

```bash
composer require dillingham/nova-attach-many
```

### Usage

```php
use NovaAttachMany\AttachMany;
```
```php
public function fields(Request $request)
{
    return [
        AttachMany::make('Permissions'),
    ];
}
```

You can explicitly define the relationship & Nova resource:

```php
AttachMany::make('Field Name', 'relationshipName', RelatedResource::class);
```

### Display on detail:

This package only provides the create / edit views that BelongsToMany does not.

BelongsToMany should be used for displaying the table on detail views.

```php
public function fields(Request $request)
{
    return [
        AttachMany::make('Permissions'),
        BelongsToMany::make('Permissions'),
    ];
}
```

### Validation

You can set min, max, size or custom rule objects

```php
->rules('min:5', 'max:10', 'size:10', new CustomRule)
```

<img src="https://user-images.githubusercontent.com/29180903/52160802-9ee9ac80-2689-11e9-9657-80e3c0d83b27.png" width="75%" />

---

### Exclusive Items

You can specify a list of IDs that, if selected, will make the selection exclusive—no other items can be selected at the same time. This is useful for options like "None" or "Unlimited" that should not be combined with other selections.

**How to use:**

```php
AttachMany::make('Permissions')
    ->exclusiveItems([1, 2, 3]);
```

- If an exclusive item is selected, all other selections are cleared and only the exclusive item remains selected.
- If any exclusive item is selected, all other checkboxes are disabled.
- If a non-exclusive item is selected, exclusive items become disabled.
- The UI shows a small message next to disabled checkboxes to clarify why they are disabled.

You can pass any array of IDs (integers or strings) to `exclusiveItems()`.

### Options

Here are a few customization options

- `->showCounts()` Shows "selected/total"
- `->showPreview()` Shows only selected
- `->hideToolbar()` Removes search & select all
- `->height('500px')` Set custom height
- `->fullWidth()` Set to full width
- `->showRefresh()` Request the resources again
- `->showSubtitle()` Show the resource's subtitle
- `->help('<b>Tip:</b> help text')` Set the help text
- `->exclusiveItems([1,2,3])` Make listed IDs exclusive: if one is selected, no others can be selected at the same time

### All Options Demo
<img src="https://user-images.githubusercontent.com/29180903/53781117-6978ee80-3ed5-11e9-8da4-d2f2408f1ffb.png" width="75%"/>

### Relatable
The attachable resources will be filtered by relatableQuery()
So you can filter which resources are able to be attached

### Being Notified of Changes

You can add a method to the resource to be notified of the changes that have happened:

The method must be a camel cased version of the attribute name, followed by `Synced`. For example:

```php
public function fields(Request $request)
{
    return [
        AttachMany::make('Permissions'),
    ];
}
```

```php
public function permissionsSynced(array $changes)
{
    $changes['attached']; // An array of IDs of attached models
    $changes['detached']; // An array of IDs of detached models
    $changes['updated']; // An array of IDs of updated models
}
```


### Authorization
This field also respects policies: ie Role / Permission
- RolePolicy: attachAnyPermission($user, $role)
- RolePolicy: attachPermission($user, $role, $permission)
- PermissionPolicy: viewAny($user)

---

# Author

Hi 👋, Im Brian D. I created this Nova package [and others](https://novapackages.com/collaborators/dillingham)

Hope you find it useful. Feel free to reach out with feedback.

Follow me on twitter: [@im_brian_d](https://twitter.com/im_brian_d)
