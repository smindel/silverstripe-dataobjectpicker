# DATAOBJECTPICKER

## Maintainer Contact
 * Andreas Piening <andreas (at) silverstripe (dot) com>

## Description

Formfield for picking one existing DataObject for the forms record, using a configurable dropdown like autosuggest list to pick from.

## Requirements
 * SilverStripe 2.4 or newer

## Installation

 1. follow the usual [module installation process](http://doc.silverstripe.org/modules#installation)

## Setup

If the owning class has the static $has_one var set and the class to pick from (target class) is propperly setup with the following static variables, DataObjectPicker does the rest for you: 
- $searchable_fields
- $summary_fields
- $default_sort

### Example #1

Auto setup example for Page which has a static $has_one Author (=> Member class):

	new DataObjectPicker('AuthorID');

The above creates a DataObject picker field. Our custom Page has a static $has_one realtion called 'Author' to the Member class. Thats how the target class is resolved that we will pick existing objects from. When entering the first characters into the field, it fires of AJAX requests to the completeFunction, which is by default DataObjectPicker's own getSuggestions(). It uses the target classes static $searchable_fields to search in using the predifined searchPattern. If $searchable_fields is not set it falls back to Title then Name then fails. The first 20 ($limit) records found will be returned JSON encoded, using the target classes static $summary_fields, ordered by the target classes static $default_sort

If this doesn't work for you because e.g. you are not in a getCMSFields() context or the target class is not set up with all the statics or you want to use your own suggestion callback you can setup almost everything like this:

### Example #2

Custom setup

	$field = new DataObjectPicker($name, $title, $value, null, $form);
	$field->setConfig('join', $yourjoinstatement);
	$field->setConfig('fieldsToSearch', array(
		'Surname',	// search for search term in Surname using the default search pattern
		'Description' => "\"TargetClassName\".\"Description\" LIKE '%%%s%%'",	// search for term in Desciption using the custom search pattern
		'Age' => "\"TargetClassName\".\"Age\" >= \"JoinedClass\".\"SomeField\"", // not actually a search but showing what is possible
	));
	$field->setConfig('completeFunction', array('Page', 'myCustomSuggestionCallback')); send the request off to Page::myCustomSuggestionCallback() for suggestions

## Feedback

Please help me to improve this module by submitting your feedback/bug reports/support requests/suggestions. Thanks