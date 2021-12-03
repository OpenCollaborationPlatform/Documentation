
Datatypes
---------
DataTypes in the dml language are a mix of standart POD types and some special dml
ones required for the functionality of distributed datastructures.

.. note:: The dml types are used for storage, in JS and via WAMP api. This includes
		  serialization, and recreation in of data in your language of choice. Therefore some
		  difference exists on how the types work when storing data, accessing from
		  JavaScript or the WAMP Api.

As a language for datastorage, dml can store and handle all JSON serializable datatypes. But not all
of those possibilities are exposed as datatypes. This comes from the fact, that the concrete types are intended
for use in defining configuration properties of Objects and Behaviours, which are to be set by the user. This process
is simplified if the type of those properties is clear to the user, and error checking can be done on
his input. For data storage use with your application you most likely want to use the the :dml:type:`var` type,
as it is more versataile.

.. note:: The dml type system was created on the fly as needed. Do not expect a perfectly reasonable and always consistent
		  language architecture around them.

Plain old datatypes
^^^^^^^^^^^^^^^^^^^

.. dml:type:: int

	Integer datatype with 64bit size. All other integer sizes are converted to
	64 bit internally. Larger integers are not supported. In JS, and hence JSON
	searialization, this is converted to 'number' type

.. dml:type:: float

	Floating point number with 64bit size. All other floating point sizes are
	converted to 64 bit

.. dml:type:: bool

	Boolean datatype, can be true or false.In JS, and hence JSON
	searialization, this is converted to 'boolean' type

.. dml:type:: string

	String datatype. Same in JA and JSON.

Special dml types
^^^^^^^^^^^^^^^^^

 .. dml:type:: var

	Variable datatype, can be anything. Note that it can hold any DML type, but even
	more. It can hold anything that is JSON searializable, like Lists or Maps. The purpose
	of this type is to enable maximal flexible datastorage for the user. This type is
	what you want to use for most of your storage needs.

	.. note:: There is no performance benefit or penaulty for using discrete types over var.

.. dml:type:: key

	Key describes a subset of all dml types, describing those that can be used as a key.
  	This includes :dml:type:`string`, :dml:type:`int`, :dml:type:`bool`, and :dml:type:`raw`.
	This type is used by datatypes that have user definable keys like :dml:prop:`Map.key`

.. dml:type:: raw

	This datatype describes raw binary data. It is exposed as CID in a property raw.
	The CID itself does not have any methods or useful interaction options, but can be
	used together with the WAMP api to retrieve the raw data.

DML types for type handling
^^^^^^^^^^^^^^^^^^^^^^^^^^^

 .. dml:type:: type:

	This type describes all types. Allowed values are all possible types as defined
	in dml, e.g. string, int, raw. It is used in Objects to specify the types used for
	special functionality, like the value of a Map.

 	DML allows to create custom types as composites from objects. That means that any
	valid dml Description like Data{} can be used as a new type, and is a valid argument
	for 'type' type. With this it is possible to use composite types e.g. as value for a
	Map.

	When accessing in JS or serializing the string representation of the datatype is
	provided. This is the type name for most types, and a encoded string in case of
	a composite type. Those strings can also be used to set a type proeprty via WAMP api.

.. dml:type:: none

	A none type, not meaning anything. It is the type version of JS undefined. The use
	case for this is to be able to not specify anything for type properties if not wanted.
	Some Objects may use this possibility to disable functionality.


