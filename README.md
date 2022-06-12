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


- ###
