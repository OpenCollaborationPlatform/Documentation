
Systems
-------



.. dml:system:: Transaction

	The transaction system provides access control for data objects. A user can open
	a transaction and make data objects part of it, and other users are then prevented
	from chaning whatever is part of the transaction.

	The general idea is to give the user a way to make multiple successive manipulations
	on the datastructure, possibly over long time with many calls, in an atomic way.
	For this he needs the gurantee, that only his calls change certain data, and
	noone else can make any unforseen changes he needs to react to. To achieve this
	the manipulated data can be made part of a transaction, which prevents anyone else
	to manipulate it.

	To enable data objects for transaction use it needs to have one of the transaction
	behaviours added. In those behaviours the detailed handling of the Object withing the
	transaction can be specified.

	.. note:: The dml transactions are different to SQL transactions as they are open
			  for successive user calls, without a time limit. This does however also
			  mean, that the user needs to take care of closing it correctly.


	
	.. dml:function:: IsOpen()
	
		Checks if the user has currently a transaction open
	
		:return bool open: True if a transaction is open
	
	

	
	.. dml:function:: Open()
	
		Opens a transaction. If one is already open, it will be closed first.
	
	

	
	.. dml:function:: Close()
	
		Closes the currently open transaction.
	
	

	
	.. dml:function:: Abort()
	
		Aborts the current transaction and reverts all objects to the state they had when adding to the transaction
	
	

