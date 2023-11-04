
```
-- DELETE target, target_path;
-- Ensures that (caption, project_id) is unique
DEFINE INDEX target_idx ON target FIELDS caption, project_id UNIQUE;

-- Create records in the target table
CREATE target SET caption = "Teaching Components", project_id=1;
CREATE target SET caption = "Teaching Components", project_id=2;

-- Records must not have the same fields if we do not want to.
-- If we want to enforce a schema
-- DEFINE TABLE target SCHEMAFULL;
-- DEFINE FIELD caption ON target TYPE string;
-- DEFINE FIELD project_id ON target TYPE int;
CREATE target SET caption = "Positive", target_type="polarity";

-- Add tags to target record. There does not seem to be a way to add things to tags. Need to fully change it
UPDATE (select value id from target where caption='Positive' limit 1) set tags={'keywords':'asdf asdf', 'examples':'qwerqwer'};


-- Forces a field target on the table target_path
DEFINE FIELD targets ON target_path TYPE array<record>;

-- This fails because there is no field targets
CREATE target_path SET project_id=1;

-- For now the order of the targets is random. Cannot enforce it
CREATE target_path SET
    targets = (
        SELECT VALUE id FROM target
        WHERE
        (caption in ["Teaching Components", "Positive"])
    )
;

SELECT * FROM target_path;
SELECT * FROM target_path FETCH targets;

SELECT * FROM target;
SELECT * FROM target WHERE target_type="polarity";
```



```
DELETE target, target_path;

-- Ensures that (caption, project_id) is unique

DEFINE INDEX target_idx ON target FIELDS caption, project_id UNIQUE;

  

-- Create records in the target table

CREATE target SET caption = "Teaching Components", project_id=1;

CREATE target SET caption = "Teaching Components", project_id=2;

-- Records must not have the same fields if we do not want to.

-- If we want to enforce a schema

-- DEFINE TABLE target SCHEMAFULL;

-- DEFINE FIELD caption ON target TYPE string;

-- DEFINE FIELD project_id ON target TYPE int;

CREATE target SET caption = "Positive", target_type="polarity";

CREATE target SET caption = "Positive", target_type="polarity", project_id=1;

  

-- Add tags to target record. There does not seem to be a way to add things to tags. Need to fully change it

-- Note that this updates both Positive target

update (select value id from target where caption='Positive' limit 1) set tags={'keywords':'asdf asdf', 'examples':'qwerqwer'};

  

-- Forces a field target on the table target_path

DEFINE FIELD targets ON target_path TYPE array<record>;

  

-- This fails because there is no field targets

CREATE target_path SET project_id=1;

  

-- For now the order of the targets is random. Cannot enforce it

CREATE target_path SET

project_id=1,

captions = ["Teaching Components", "Positive"],

targets = (

SELECT VALUE id FROM target

WHERE

(

caption in ["Teaching Components", "Positive"]

AND project_id=1

)

)

;

  

LET $target_number = 1;

LET $tp = (SELECT VALUE id FROM target_path LIMIT 1);

FOR $caption IN ["Teaching Components", "Positive"] {

LET $target = (SELECT VALUE id FROM target where caption=$caption and project_id=1);

RELATE $tp->contains_target->$target SET idx = $target_number;

$target_number = $target_number + 1;

};

  
  

SELECT * FROM contains_target;

  
  

SELECT * FROM target_path;

SELECT * FROM target_path FETCH targets;

  

SELECT * FROM target;

SELECT * FROM target WHERE target_type="polarity";

select value caption from target order by caption;

select * from target order by caption;

  

SELECT VALUE id FROM target WHERE caption="Positive";

  
  

LET $tt = [];

--$targets = array::concat($targets, ["asdf"]);

LET $project_id = 1;

FOR $caption IN ["Teaching Components", "Positive"] {

$project_id = $project_id + 1;

--$targets = array::concat($targets, [$caption]);

};

CREATE target_path SET

project_id=$project_id,

targets = $tt

;

  
  

LET $persons = [];

FOR $names IN ["Bob", "Alice"] {

$persons = $persons + (SELECT VALUE id FROM person WHERE caption=$caption);

};
```

```rust
DELETE target, target_path, contains_target, is_linked;

-- Ensures that (caption, project_id) is unique

DEFINE INDEX target_idx ON target FIELDS caption, project_id UNIQUE;

  

DEFINE FIELD caption ON target TYPE string;

-- int or DIE HARD

DEFINE FIELD project_id ON target TYPE option<int>;

  

LET $project_id=1;

  

-- Define a few targets

CREATE target SET caption = "Exams", target_type='category', project_id=$project_id;

CREATE target SET caption = "Grading", target_type='category', project_id=$project_id;

CREATE target SET caption = "Clarity", target_type='attribute', project_id=$project_id;

CREATE target SET caption = "Fairness", target_type='attribute', project_id=$project_id;

CREATE target SET caption = "Positive", target_type="polarity", project_id=$project_id;

CREATE target SET caption = "Negative", target_type="polarity", project_id=$project_id;

SELECT * FROM target;

-- Forces a field target on the table target_path

--DEFINE FIELD targets ON target_path TYPE array<record>;

  

FOR $category IN (SELECT * FROM target WHERE target_type='category') {

FOR $attribute IN (SELECT * FROM target WHERE target_type='attribute') {

FOR $polarity IN (SELECT * FROM target WHERE target_type='polarity') {

LET $tp = CREATE ONLY target_path

SET caption = string::join(

'>',

$category.caption,

$attribute.caption,

$polarity.caption

);

RELATE ($tp.id)->contains_target->($category.id) SET idx=1;

RELATE ($tp.id)->contains_target->($attribute.id) SET idx=2;

RELATE ($tp.id)->contains_target->($polarity.id) SET idx=3;

  

RELATE ($category.id)->tp_link->($attribute.id) SET tp=$tp.id;

RELATE ($attribute.id)->tp_link->($polarity.id) SET tp=$tp.id;

}

}

};

  

SELECT *, (

SELECT in as target_path, out as target, idx from contains_target where in = $parent.id order by idx

) as targets

FROM target_path

WHERE ->contains_target->(target where caption='Positive')

order by targets.idx

FETCH targets, targets.target;

  
  

SELECT *

FROM target

WHERE ->tp_link->(target where caption='Positive');

  

SELECT *

FROM target

WHERE ->tp_link->(target where caption='Clarity')->tp_link->(target where caption='Positive');

  

SELECT * FROM tp_link

WHERE tp IN (

SELECT value tp FROM tp_link where out.caption="Positive"

)

ORDER BY tp

FETCH in, out;
```