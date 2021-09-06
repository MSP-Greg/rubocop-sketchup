# SketchupPerformance

<a name='openssl'></a>
## SketchupPerformance/OpenSSL

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

There are performance issue with the OpenSSL library that Ruby ship. In
a clean SU session, default model there is a small delay observed in the
Windows version of SU.

But with a larger model loaded, or session that have had larger files
loaded the lag will be minutes.

`SecureRandom` is  also affected by this, as it uses OpenSSL to seed.

It also affects `Net::HTTP` if making HTTPS connections.

### References

* [https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#openssl](https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#openssl)

<a name='operationdisableui'></a>
## SketchupPerformance/OperationDisableUI

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Operations should disable the UI for performance gain.

### Examples

```ruby
model = Sketchup.active_model
model.start_operation('Operation Name', true)
# <model changes>
model.commit_operation
```

### References

* [https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#operationdisableui](https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#operationdisableui)

<a name='selectionbulkchanges'></a>
## SketchupPerformance/SelectionBulkChanges

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Prefer changing selection in bulk instead of modifying selection within
loops. It's much faster to change the selection in bulk. UI updates are
triggered when you update the selection, so reduce the amount of calls.

### Examples

#### Poor performance

```ruby
model = Sketchup.active_model
model.active_entities.each { |entity|
  model.selection.add(entity) if entity.is_a?(Sketchup::Face)
}
```
#### Better performance

```ruby
model = Sketchup.active_model
faces = model.active_entities.map { |entity|
  entity.is_a?(Sketchup::Face)
}
model.selection.add(faces)
```
#### Better performance and simpler

```ruby
model = Sketchup.active_model
faces = model.active_entities.grep(Sketchup::Face)
model.selection.add(faces)
```

### References

* [https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#selectionbulkchanges](https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#selectionbulkchanges)

<a name='typecheck'></a>
## SketchupPerformance/TypeCheck

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

String comparisons for type checks are very slow.

`entity.class.name == 'Sketchup::Face'` is slow because it performs a
string comparison. `is_a?` is much faster because it's a simple type
check.

If you know you need a strict type check, compare the classes directly:
`entity.class == Sketchup::Face`.

### Examples

#### Poor performance

```ruby
entity.class.name == 'Sketchup::Face'
```
#### Good performance

```ruby
entity.class == Sketchup::Face
```
#### Good performance and idiomatic Ruby convention

```ruby
entity.is_a?(Sketchup::Face)
```

### References

* [https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#typecheck](https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#typecheck)

<a name='typename'></a>
## SketchupPerformance/Typename

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

`.typename` is very slow, prefer `.is_a?` instead.

`entity.typename == 'Face'` is slow because it performs a string
comparison. `is_a?` is much faster because it's a simple type check.

### References

* [https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#typename](https://github.com/SketchUp/rubocop-sketchup/tree/main/manual/cops_performance.md#typename)
