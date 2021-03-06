# Metadata

* [What are metadata?](#markdown-header-what-are-metadata)  
* [How to use?](#markdown-header-how-to-use)
* [Creating metadata](#markdown-header-creating-metadata)
* [Updating metadata](#markdown-header-updating-metadata)
* [Deleting metadata](#markdown-header-deleting-metadata)
* [Retrieving metadata](#markdown-header-retrieving-metadata)
* [What else?](#markdown-header-what-else)
* [IMPORTANT!](#markdown-header-important)  

## What are metadata?
As their name means, these are additional informations about an existing entity. The difference between them and **variables** is that these are kind of permanent informations that do get updated but not as often as variables do.  

For example, you can use metadata to store users phone numbers; address; website; biographies; social links ... etc.  
You can use them to store products prices; products wishlists; users, objects or groups extra details that cannot be put inside their respective tables.

## How yo use?  
There are four (**4**) important methods (or functions if you want) that you can use:  
* **create()** (or *add_meta()*);
* **update()** (or *update_meta()*);
* **get()** (or *get_meta()*);
* **delete()** (or *delete_meta()*).

There is another method available that you can use but we will leave it for later.  

### Creating Metadata:
After creating the entity you want (user, group or object), you can (MUST) use its ID to create metadata for it. In your controllers, you can use:  
```php
// To create a single metadata.
$this->app->metadata->create($guid, $name, $value);
// Or you can use the function:
add_meta($guid, $name, $value);
// $guid here is the entity's ID.
```
If you want to create multiple metadata for the selected entity, you only need to pass an associative array as the second parameter like so:
```php
$this->app->metadata->create($guid, array(
	'phone' => '0123456789',
	'company' => 'Company Name',
		'location' => 'Algeria',
		'address', // (1)
		'website', // (2)
), $value);
// "address" and "website" have not values, so they will be
// created by but they will use the third argument ($value) 
// as a value. Default: NULL
```
### Updating Metadata:
To update a metadata, you can use either the **update** method or the **update_meta** function. So in your controllers, you can do like the following:  
```php
// To update a single metadata.
$this->app->metadata->update($guid, $name, $value);
// Or use the helper function:
update_meta($guid, $name, $value)
```
To update multiple metadata, pass an array as the second parameter like so:
```php
$this->app->metadata->update($guid, array(
	'phone' => '0987654321',
	'company' => 'New Company',
	'address', // <- Same as create, it will use $value
	'website' => 'https://www.ianhub.com/'
), $value);
// Or use the helper function:
update_meta(...)
```
**NOTE**: This method or its helper will not only update the selected metadata but they will create it if it does not exists. So you can use it to create metadata as well.  

If you want to update a single or multiple metadata by arbitrary _WHERE_ clause, you can use the following method:
```php
$this->app->metadata->update_by($where, $data);
// Or the helper:
update_meta_by($where, $data);
```
**Note**: if you pass only **where**, it will be treated as the **data** to updated and all metadata will be updated.

### Deleting Metadata:
In order to delete metadata, you can use the method **delete** or the helper **delete_meta**. So in your controllers, you may have:
```php
// Delete a single metadata.
$this->app>metadata($guid, $name);
// Or if you use the helper:
delete_meta($guid, $name);
```
To delete multiple metadata, you have two options, pass an array as the second parameter, or cascade parameters like the example below:  

Let's say I want to delete "phone" and "company".
```php
// Option 1:
$this->app->metadata->delete($guid, ['phone', 'company']);
// Option 2:
$this->app->metadata->delete($guid, 'phone', 'company');

// You can use the helper function:
delete_meta($guid, ['phone', 'company']);
```
### Retrieving Metadata:
You can retrieve metadata by using the **get** method or the **get_meta** function. Theses functions accept three (**3**) arguments:  
* **$guid**: (*int*) which is the entity's ID.
* **$name**: (*string*) if empty, you will get all metadata for that entity; if you pass a string, you will get the selected metadata.
* **$single**: (*boolean*) this is useful in cas you want to retrieve the value of metadata directly instead of retrieving the whole object.

So in your controllers, you would have:
```php
// The following code will retrieve the phone object from
// database if found, otherwise it returns NULL.
$meta = $this->app->metadata->get($guid, 'phone');
// Or the helper:
$meta = get_meta($guid, 'phone');
    
// In this case, to retrieve the phone number you do:
$phone = $meta->value; // Outputs: '0123456789' if found.
```
If you want to retrieve the value instead of the object, you can do:  
```php
$phone = $this->app->metadata->get($guid, 'phone', TRUE);
// Or the helper:
$phone = get_meta($guid, 'phone', TRUE);    
// Here, $phone = '0123456789' for example.
```
If you want to retrieve all metadata of the selected entity, just use its ID and ignore other arguments:
```php 
$metadata = $this->app->metadata->get($guid);
// Or the helper:
$metadata = get_meta($guid);
```
There is additional methods and their helpers that you can use to retrieve a single or multiple metadata by arbitrary _WHERE_ clause: **get_by** and **get_many**.  
```php 
// To retrieve a single metadata:
$this->app->metadata->get_by($field, $match);
// Or its helper:
get_meta_by($field, $match);

// To retrieve multiple metadata:
$this->app->metadata->get_many($field, $match);
// Or its helper:
get_many_meta($field, $match);
```
## What else?
Sometimes users, objects or groups are deleted from **entities** table. And because they no longer exist, it is kind of useless to keep their metadata, this is why the **purge** method or **purge_meta** function were added.  
These funtions will simply delete ALL metadata of entity's that do not exist. That's all. To use them:
```php 
$this->app->metadata->purge();
// Or the helper:
purge_meta();
```
## IMPORTANT:
All methods and functions are to be used inside controllers. In case you want to use them inside libraries, make sure to never use helpers because they will trigger an `undefined property: $app`  error.
