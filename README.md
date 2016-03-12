# Custom validation with $validators

## Overview

The built-in validation in HTML forms can only go so far. However, we can actually define custom validation rules!

## Objectives

- Describe the $validators pipeline
- Write a custom validator
- Integrate the custom validator with ngMessages

## $validators

Much like `$parsers` and `$formatters`, we add our custom validators into `$validators`. Here we have a function where we can test if the value is valid, returning true or false depending on their validity.

Let's create a directive to see if our input is *only* numbers.

```js
function integer() {
	return {
		restrict: 'A',
		require: 'ngModel',
		link: function (scope, element, attrs, ngModel) {
			ngModel.$validators.integer = function (value) {
				return value.test(/^\-?\d+$/);
			};
		}
	}
}

angular
	.module('app')
	.directive('integer', integer);
```

You might've noticed we're simply adding to the `ngModel.$validators` object instead of pushing into it. This is because we need to give our validators a simple name that will then get added to our `$errors` object if the validation returns false. We can also use these custom validators in `ngMessages`!

Example usage of our new validator would look like this:

```html
<form name="form">
	<input name="number" ng-model="ctrl.number" />

	<div ng-messages="form.number.$errors">
		<div ng-message="integer">Number must be an integer!</div>
	</div>
</form>
```

Awesome! Custom validation that slots in nicely with the built-in validation we've used previously.