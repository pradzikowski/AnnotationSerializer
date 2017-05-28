
# TODO:
- [ ] Simple Annotation
- [ ] Simple annotation MetaReader class
- [ ] Simple Serializator class
- [ ] Simple Unserializator class with Object factory
- [ ] Complex Annotation
- [ ] MetaInfo class which hold all annotation definitions
- [ ] Complex annotation MetaReader with MetaInfo factory
- [ ] Translate MetaInfo object data into serializator and unsezalizator
- [ ] Complex serialize process
- [ ] Complex unserialize process
- [ ] Complex object factory based on MetaInfo
- [ ] Multiple serialize format ex: XML, Json

Examples:
Simple Annotation:

```php
<?php

use PCF\AnnotationSerializer\Annotation\Field;

class User {

    /*
     * @Field("username")
     */
    public $username;

    /*
     * @Field("familyname")
     */
    public $surname;

}

$reader = new \Doctrine\Common\Annotations\AnnotationReader;
$metaReader = new \PCF\AnnotationSerializer\MetaReader($reader);
$meta = $metaReader->getClassMetaData(User::class);

$user = new User;
$user->username = 'Paweł';
$user->surname = 'Radzikowski';

$serializedData = \PCF\AnnotatdssdfdsfionSerializer\Serializer::serialize($meta, $user);
```

should produce object:

``` php
//array
[
    "username"=> "Paweł",
    "familyname"=> "Radzikowski"
]

```

Complex Annotation:

```php
<?php

use PCF\AnnotationSerializer\Annotation\Field;
use PCF\AnnotationSerializer\Annotation\Serializable;
use PCF\AnnotationSerializer\Annotation\Property;
use PCF\AnnotationSerializer\Annotation\Collection;
use PCF\AnnotationSerializer\Annotation\Collection;

/*
 * @Serializable
 * @Field(
 *   type="string",
 *   value=@Property("rolename")
 * )
 */
class Role {
    public $rolename;
}

/**
 * @Serializable
 */
class User {
    /*
     * @Field("username")
     */
    public $username;

    /*
     * @Field("familyname")
     */
    public $surname;

    /*
     * @Field(
     *  name="roles",
     *  type=@Collection(
     *    type=@Class("Role")
     *  )
     * )
     *
     */
    public $roles = [];
}

$reader = new \Doctrine\Common\Annotations\AnnotationReader;
$metaReader = new \PCF\AnnotationSerializer\MetaReader($reader);
$meta = $metaReader->getClassMetaData(User::class);

$user = new User;
$user->username = 'Paweł';
$user->surname = 'Radzikowski';


$role = new Role;
$role->name = 'Admin';

$user->roles = [
    $role,
];

$serializedData = \PCF\AnnotatdssdfdsfionSerializer\Serializer::serialize($meta, $user);

```

should produce object:

``` php
//array
[
    "username"=> "Paweł",
    "familyname"=> "Radzikowski"
    "roles" => [
        "Admin"
     ]
]

```
