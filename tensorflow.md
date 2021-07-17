# tensorflow

## issue `#290`

Issue [`#290`](https://github.com/tensorflow/rust/issues/290)

### what needs to be done?

There is a struct called `OpArgDef` in `src/lib.rs` LN: 1714
```rust
pub struct OpArgDef {
    name: String,
    description: String,
    field_type: DataType,
    type_attr: String,
    number_attr: String,
    type_list_attr: String,
    is_ref: bool,
	+ default_value: AttrValue,
	+ allowed_values: AttrValue,
}
```

To fill the fields `default_value` and `allowed_values` in `OpArgDef` we need a new enum called `AttrValue`.
This `AttrValue` enum looks like the enum `AttrValue_oneof_value`:
```rust
pub enum AttrValue_oneof_value {
    s(::std::vec::Vec<u8>),
    i(i64),
    f(f32),
    b(bool),
    field_type(super::types::DataType),
    shape(super::tensor_shape::TensorShapeProto),
    tensor(super::tensor::TensorProto),
    list(AttrValue_ListValue),
    func(NameAttrList),
    placeholder(::std::string::String),
}
```

Do I need add/remove any variants from this enum to in `AttrValue` or can I take it as it is? Where do I need to create this enum?
Should I create it in `src/lib.rs`?

Here, the two new fields are from the struct `OpDef_AttrDef` that are present in `src/protos/op_def.rs` LN: 1176
```rust
pub struct OpDef_AttrDef {
    // message fields
    pub name: ::std::string::String,
    pub field_type: ::std::string::String,
    pub default_value: ::protobuf::SingularPtrField<super::attr_value::AttrValue>,
    pub description: ::std::string::String,
    pub has_minimum: bool,
    pub minimum: i64,
    pub allowed_values: ::protobuf::SingularPtrField<super::attr_value::AttrValue>,
    // special fields
    pub unknown_fields: ::protobuf::UnknownFields,
    pub cached_size: ::protobuf::CachedSize,
}
```

## 1st febru

We have a `struct` called `AttrValue`:
```
AttrValue --> field: Value
					 |--> datatype: Option<AttrValue_oneof_value>
```

We have an `enum` called `AttrValue_oneof_value`:
```
AttrValue_oneof_value --> numerous variants
						|--> 

## 1st march
