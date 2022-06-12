# laravel-admin-issues


- ### Fixes Array to string conversion - when use MultipleSelect | Checkbox
  - Add to class model (Replace "Location","Pictures" to inputName)
```
public function getLocationAttribute($value)
{
	return explode(',', $value);
}

public function setLocationAttribute($value)
{
	$this->attributes['location'] = implode(',', $value);
}
```
OR
```
public function getPicturesAttribute($pictures)
{
    if (is_string($pictures)) {
        return json_decode($pictures, true);
    }

    return $pictures;
}

public function setPicturesAttribute($pictures)
{
    if (is_array($pictures)) {
        $this->attributes['pictures'] = json_encode($pictures);
    }
}
```
More:
- https://github.com/z-song/laravel-admin/issues/1195
- https://github.com/z-song/laravel-admin/issues/2379

#
- ### Nested parent child on Tree class
	- Add to class model
```
public function parent()
{
	return $this->belongsTo('App\Models\Category', 'parent_id');
}

public function children()
{
	return $this->hasMany('App\Models\Category', 'parent_id');
}

public function childrenRecursive()
{
	return $this->children()->with('childrenRecursive');
}
```

#
- ### Delete with Relationship
	-  Add to model
	```
	protected static function booted()
    {
        static::deleting(function (Category $category) {
            $category->seo()->delete();
        });
    }
	```
	- Add to method form() - controller
	```
	$form->saving(function (Form $form) {
		if (blank($form->_order)) {
			if (blank($form->input('seo.title'))) {
				$form->input('seo.title', Str::substr($form->title, 0, 65));
			}
		}
	});
	```
	
#	
