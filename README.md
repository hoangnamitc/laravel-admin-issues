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

- https://github.com/z-song/laravel-admin/issues/1195#issuecomment-330110024 
- https://github.com/z-song/laravel-admin/issues/2379#issuecomment-416175367 

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

  - Add to model

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
	if (blank($form->slug) && filled($form->title)) {
		$form->slug = Str::slug($form->title);
	}

	if (blank($form->_order)) {
		if (blank($form->input('seo.title'))) {
			$form->input('seo.title', Str::substr($form->title, 0, 65));
		}
	}
});
```

#

- ### Thumbnail

	- <https://github.com/z-song/laravel-admin/pull/3453#issuecomment-501730548>

#
- ### remove image multiple
![remove-ImageMultiple](https://user-images.githubusercontent.com/7042462/173233469-e7dcbdc4-6d82-48ba-bbe9-f991484de93d.svg)

#
