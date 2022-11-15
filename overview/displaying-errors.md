# Displaying Errors

After validation you can use the same results object and use it to display the validation errors in your client side:

## Handlers:

```javascript
// store the validation results in the request collection
prc.validationResults = validate( obj );
```

## Views:

```markup
<-- Display all errors as a message box --->
#getInstance( "MessageBox@cbMessagebox" )
    .renderMessage( type="error", messageArray=prc.validationResults.getAllErrors() )#
```

If you want more control you can use the `hasErrors()` and iterate over the errors to display:

```javascript
<cfif prc.validationResults.hasErrors()>
    <ul>
    <cfloop array="#prc.validationResults.getErrors()#" index="thisError">
        <li>#thisError.getMessage()#</li>
    </cfloop>
    </ul>
</cfif>
```

You can even use the results object in your views to get specific field errors, message, etc.

## Functional Approach

You can also use the callbacks `onError() and onSuccess` to finalize the validation.  These are very common when using non only html apps but api apps.

```javascript
any function saveShared( event, rc, prc ){
		// validation
		validate(
			target      = rc,
			constraints = "sharedUser"
		).onError( function( results ){
			flash.put(
				"notice",
				results.getAllErrors().tostring()
			);
			return index( event, rc, prc );
		})
		.onSuccess( function( results ){
			flash.put( "User info validated!" );
			relocate( "main" );
		} );
	}
```

## Common Methods

The following are some common methods from the validation result object for dealing with errors:

* `getResultMetadata()`
* `getFieldErrors( [field] )`
* `getAllErrors( [field] )`
* `getAllErrorsAsJSON( [field] )`
* `getAllErrorsAsStruct( [field] )`
* `getErrorCount( [field] )`
* `hasErrors( [field] )`
* `getErrors()`
* `onError( consumer )`
* `onSuccess( consumer )`

The API Docs in the module (once installed) will give you the latest information about these methods and arguments.
