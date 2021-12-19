
API
---


.. dml:object:: Data

	The most basic implementation of a DML Object. It allows to add properties, events
	and functions and can hold other Objects and Behaviours as children. It has no other
	special functionality. It is intended as dml grouping 	object as well as base object
	for all other data types
	Data does allow for children. Note that children are static values, they cannot change at
	runtime. Hence they are different to dynamic objects as are possible with Maps etc. Children are
	used purely for the static DML hirarchy.

	.. dml:property:: name
		:const:
		:type: string

		A property defining the name of the object. The name can than be used to access in
		the hirarchy, either in JavaScript code or as WAMP uri. It is mandatory to set the name
		of each object.

	.. dml:property:: parent
		:const:
		:type: Data

		The parent object of the object. The value is null for the toplevel object.

	.. dml:property:: children
		:const:
		:type: [Data]

		A list of all children the Data object has. Note that this are only the children defined
		in the DML file itself, not the dynamic subobjects some types can add, like maps entries.

	.. dml:property behaviours
		:const:
		:type: [Behaviour]

		A list of all behaviours the Data object has attched to it.

	.. dml:event:: onBeforePropertyChange

		Emitted bevore a property of the object changes. At time of emit the
		property still has its old value.

		:argument string Property: Name of the property thats about to be changed

	.. dml:event:: onPropertyChanged

		Emitted when a property was changed. The value of the property is already the
		new one when emitted.

		:argument string Property: Name of the property thats was changed

	.. dml:event:: onCreated

		Emitted after the object was created and fully setup. It will be emitted
		only dynamically created objects, for example when used as value in Maps,
		and not the ones in the static DML hirarchy.

	.. dml:event:: onRemove

		Emitted after the object is about to be removed. At the time of emitting
		the object is still fully setup and accessible, the removing will happen after
		all handlers have been executed.
		As static hirarchy objects cannot be removed this event will be emitted only
		for dynamically created objects, for example when used as value in Maps etc.

	.. dml:event:: onBeforeChange

		This is a general event, emitted bevore the object itself changes, no matter
		what the changes are. This is not emitted for changed properties, there is a
		custom event for that, but if the objects content is manipulated. This means that
		for a Data object it will never be emitted, as it does not have any object
		content, but it may be emitted for derived object types like maps.

		.. note:: Most derived classes that emit this event will also have custom
				  events that are more specialized for the eexact changes that happend.

	.. dml:event:: onChanged

		This is a general event, emitted after the object has changed, no matter
		what the changes are. This is not emitted for changed properties, there is a
		custom event for that, but if the objects content was manipulated. This means that
		for a Data object it will never be emitted, as it does not have any object
		content, but it may be emitted for derived object types like maps.

		.. note:: Most derived classes that emit this event will also have custom
				  events that are more specialized for the eexact changes that happend.



.. dml:object:: Map
	:derived: Data

	Mapping from any key type to any other type. A Map is a standart datatype as
	avilable in all programming languages, sometimes nown as Dictionary.

	.. dml:property:: key
		:const:
		:type: key

		A property defining the datatype of the maps keys. Allowed values are all
		key datatypes like int and string.

		:default: string

	.. dml:property:: value
		:const:
		:type: type

		A property defining the datatype of the maps values. Allowed values are
		dml types including var. This allows for nesting objects by making the map
		value a new subtype.

		:default: none


	
	.. dml:function:: Length()
	
		Returns the length of the map,  which is defined as the number of keys.
	
		:return int length: The length of the map
	
	

	
	.. dml:function:: Keys()
	
		Provides access to all keys in the map. The type of the keys is dependend on
		the Maps :dml:prop:`key` property. If called from javascript, an unmutable array is returned,
		if called via WAMP API it will be the list type that is supported by the
		calling language  (e.g. List for python)
	
		:return List[any] keys: All keys that are in the map
	
	

	
	.. dml:function:: Has(key)
	
		Checks if the given key is available in the Map. The key must be of correct
		type, e.g. if the Map key property defines int, the key must be an integer and
		not a string describing the integer (like 1 and not "1"). This is different to
		how the Map handels WAMP Uri for accessing its values, as there the key must always
		be given as string.
	
		:throws: If key is of wrong type
		:return bool has: True if the key is available in the Map
	
	

	
	.. dml:function:: Get(key)
	
		Returns the value currently stored for the given key. If the Maps value type
		is a Object it will return the object itself when called from JavaScript, and
		the object ID if called by WAMP Api.
	
		:throws: If key is not available
		:throws: If key is of wrong type (must be equal to key property)
		:return any value: The value for the key
	
	

	
	.. dml:function:: Set(key, value)
	
		Sets the value for the given key. If already available it the old value will
		be overriden, otherwise it will be newly created and set to value. Key and value type
		need to be consitent with the Maps defining properties.
	
		If the Map value type is a object, this function will fail. It is not possible,
		to set it to a different object or override it once created. Use the :dml:func:`New` function
		for creating the object for a given key.
	
		:throws: If key is of wrong type (must be equal to :dml:prop:`key` property)
		:throws: If value is of wrong type (must be equal to :dml:prop:`value` property)
		:throws: If value type of Map is a Object
	
	

	
	.. dml:function:: New(key)
	
		Creates a new entry with the given key and sets it to the value types default,
		e.g. 0 for int, "" for string etc. If the value type is a Object it will be fully
		setup, and its onCreated event will be called. The :dml:func:`New` function is the only way to
		create a key entry if value type is an Object, as :dml:func:`Set` will fail in this case.
	
		:throws: If key already exists
		:throws: If key is of wrong type (must be equal to :dml:prop:`key` property)
		:return any value: The stored value for given key
	
	

	
	.. dml:function:: Remove(key)
	
		Removes the key from the map. If value type is a Object, its onRemove event
		will be called and afterards will  be deleted.
	
		:throws: If key does not exist
		:throws: If key is of wrong type (must be equal to :dml:prop:`key` property)
	
	

.. dml:behaviour:: Behaviour
	:abstract:

	Base class for all behaviours, adding common properties and events. It cannot be
	used directly, only behaviours derived from it. It does add the possibility to add
	custom properties,  events and functions. Children are not allowed.

	.. dml:property:: name
		:const:
		:type: string

		A property defining the name of the behaviour. The name can be used to access ut in
		the hirarchy, either in JavaScript code or as WAMP uri. It is mandatory to set the name
		of each behaviour.

	.. dml:property:: parent
		:const:
		:type: Data

		The parent object of the behaviour, the one which it extends.

	.. dml:property:: recursive
		:const:
		:type: bool

		Defines if the behaviour is applied recursively for all children and subobjects
		of the behaviours parent. For example, if a behaviour is added to a Map Object,
		it may watch for changes in that object. If recursive is true, it will also look
		for all changes in any children or value objects of that Map.

		:default: false

	.. dml:event:: onBeforePropertyChange

		Emitted bevore a property of the object changes. At time of emit the
		property still has its old value.

		:argument string Property: Name of the property thats about to be changed

	.. dml:event:: onPropertyChanged

		Emitted when a property was changed. The value of the property is already the
		new one when emitted.

		:argument string Property: Name of the property thats was changed



.. dml:behaviour:: Continuity
	:derived: Behaviour

	The continuity behaviour helps you to ensure continious updates to a object. The idea is
	to make updates on a object only possible, if they are based on the latest available data.

	To ensure this, the caller, the one who wants to make a change, need to show that he based
	his update on the currently existing object. For this, the behaviour adds the :dml:prop:`state`
	property to the object which represents the current state as a integer. When a set of changes
	is finished, the state gets incremented (by the user or automatically). Any caller needs to
	ensure, that his change is based on the currently available data by providing his last
	state as integer. If this provided known state matches the current :dml:prop:`state` value, the
	update is allowed, and if it does not match, the update fails.

	..note:: The state increment happens after all user code is executed. Calling "Increment" on a
			 continuity behaviour marks that object for state increment but does not do it directly.
			 It is only done just before finishing the WAMP api call. The same holds for automatic
			 increments.

	.. dml:property:: automatic
		:const:
		:type: bool
		:default: false

		If set to true a state increment is done automatically for each user call that changes
		the extended object. For example, if a user sets a property on the parent object via
		the WAMP api, the state of the continuity behaviour is incremented when automatic==true.
		If annother property of the parent object is changed during the same WAMP api call, the
		state is not incremented anymore. The next increment can only happen in the next WAMP api
		call or by using the manual methods of the behaviour.

	..  dml:property:: state
		:readonly:
		:type: int
		:default: 0

		A integer describing the current state of this object. Gets incremented if a new state is
		reached. This happens either by user action by calling the increment method, or automatically
		on a change if configured like this.



.. dml:behaviour:: Transaction
	:derived: Behaviour

	Defines how a Object behaves in transactions. With this behaviour defined in a Object
	it will become part of the current transaction when a change occurs. If any other user tries
	to edit it an error will be raised and the action fails.

	Any change within the object does trigger the transaction behaviour, be it a set property
	or any Object internal change, like a new entry in a Map. If recursive is true, the same
	holds for any change in a child- or subobject. Note that a change in a child will not add the
	child to the current transaction, but the Object which has the behaviour defined.


	
	.. dml:function:: Add()
	
		Adds the Object to the current transaction. If the Object is already part of the
		current transaction nothing happens, and no error is thrown.
	
		:throws: If no transaction is open for current user
		:throws: If Object is already part of a transaction of annother user
	
	

	
	.. dml:function:: InTransaction()
	
		Checks if the Object is currently part in any transaction
	
		:return bool IsPart: True if in a transaction, false otherwise
	
	

	
	.. dml:function:: InCurrentTransaction()
	
		Checks if the Object is currently part in the users transaction
	
		:return bool IsPart: True if in users transaction, false otherwise
	
	

	
	.. dml:function:: InDifferentTransaction()
	
		Checks if the Object is currently part in a transaction of annother user
	
		:return bool IsPart: True if in other users transaction, false otherwise
	
	

	
	.. dml:function:: CanBeAdded()
		:virtual:
	
		Allows custom logic for checking if the Object is allowed to take part in the
		current transaction. To be overriden and implemented by the user. If false is returned,
		the action that lead to the Object being added to the transaction fails.
		Note that it is not needed to check if the Object is already part of annother
		transaction, this is still done internally. This function is to be used for custom logic
		only.
	
		:return bool Possible: True if it is allowed to add object to current transaction
	
	

	
	.. dml:function:: CanBeClosed()
		:virtual:
	
		Allows custom logic for checking if the transaction, in which the Object takes part,
		is allowed to be closed. To be overriden and implemented by the user. If false is returned,
		closing the transaction fails and it stays open. The implementation should make sure
		that the user is informed about the reasons, so that he can correct it bevore trying
		to close the transaction again.
	
		.. note:: This function is not called for aborting transactions. Aborting cannot
				  be prevented.
	
		:return bool Possible: True if it is allowed to close the current transaction
	
	

	
	.. dml:function:: DependentObjects()
		:virtual:
	
		Allows custom logic for defining dependent Objeccts. To be overriden and implemented
		by the user. This function will be called when the Object is added to the transaction,
		and the list of returned Objects will then be added to the current transaction too.
	
		:return List[Objecct] Dependencies: All Objects that need to also be part of the transaction
	
	

	
	.. dml:event:: onParticipation
	
		Emitted when the Object becomes part of the users current transaction. If a JavaScript
		callback for this event throws an error the adding of the Object fails, as well as the
		user action triggering it. Hence this can be used the same way as overriding *CanBeAdded*
	
	.. dml:event:: onClosing
	
		Emitted when current transaction, of which the Object is a part, is closed. If a JavaScript
		callback for this event throws an error the transaction closing fails, as well as the
		user action triggering it. Hence this can be used the same way as overriding *CanBeClosed*
	
	.. dml:event:: onAborting
	
		Emitted when current transaction, of which the Object is a part, is aborted. Errors in
		callbacks ar ignored, as aborting cannot be stopped.
	
	.. dml:event:: onFailure
	
		Emitted when adding the object to the current transaction fails, independent of
		the reason for the fail.
	
		:arg str error: The error message describing why it failed
	
	

.. dml:behaviour:: PartialTransaction
	:derived: Behaviour

	Defines how the objects individual keys behaves in transactions. With this behaviour defined in a object
	its keys can become part of that transaction. A key is any identifier pointing to data, like a property
	name, a number for a Vector or a key for a map. If the behaviour is recursive keys can also be stacked,
	like childname.mapkey

	Any change within the object does trigger the transaction behaviour, be it a set property
	or any Object internal change, like a new entry in a Map. If recursive is true, the same
	holds for any change in a child- or subobject. Note that a change in a child will not add the
	child to the current transaction, but the Object which has the behaviour defined.

	.. note:: Keys are relative paths from the behaviours parent object, e.g. MyChild.myProperty


