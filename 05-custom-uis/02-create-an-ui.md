# Create an UI

Creating UI depends on two objects, `UIComponent` and `UIView` (optionally).

- **UIComponent** is the base object for an UI. It takes care of the validation, representation values, its Input view (UIView), and anything in between.
- **UIView** is a Backbone.View object that represents the Input, what the user will see while editing an entry. Anything you can do on a Backbone View you can do it on UIView. To be more specific here we are extending from [Backbone Layout Manager](https://github.com/tbranyen/backbone.layoutmanager) which extends Backbone View, so anything that backbone.layoutmanager does you can do it too.

As an example we are going to create a text area with characters limitation, and will update the remaining characters as the user add or remove characters from the text area.

## Input
 
```js
define(['core/UIComponent', 'core/UIView'], function(UIComponent, UIView) {
    var charactersLimit = 100;
    var Input = UIView.extend({        
        templateSource: '<textarea maxLength="{{maxLength}}"></textarea><span id="count">{{charactersRemaining}}</span>',
        events: {
            'keyup textarea': 'updateCount'
        },
        updateCount: function(event) {
            var textLength = event.currentTarget.value.length;
            var textRemaining = charactersLimit - textLength;
            this.$el.find('#count').text(textRemaining);
        },
        serialize: function() {
            return {
                maxLength: charactersLimit,
                charactersRemaining: charactersLimit
            }
        }
    });
});
``` 

### Template Source
Template source can be any valid [Handlebars](http://handlebarsjs.com) source, which include plain HTML and CSS.

### Events
We can bind events to any element inside the UIView, but we cannot bind to elements outside the view. if for some good reason you want to bind to a element outside, you can use the global jquery object `$`.

Listening to `keyup` events on the text area will execute `updateCount` method.

### Update Count
**uppdateCount** method will execute every time the user enter a characters in the text area and updating the counter.

### Serialize
**serialize** method returns the data that is going to be used when the **templateSource** is rendered.

## Component

```js
var Component = UIComponent.extend({
    id: 'textlimit',
    dataTypes: ['TEXT'],
    Input: Input
});
```


### ID
**id** is the UI's unique name.

### Data Types
**dataTypes** are the supported data types for the UI. In our case we want to support only `TEXT` column type.
 
### Input
Is the input view for the component.

## Complete UI code

```js
define(['core/UIComponent', 'core/UIView'], function(UIComponent, UIView) {
    var charactersLimit = 100;
    var Input = UIView.extend({        
        templateSource: '<textarea maxLength="{{maxLength}}"></textarea><span id="count">{{charactersRemaining}}</span>',
        events: {
            'keyup textarea': 'updateCount'
        },
        updateCount: function(event) {
            var textLength = event.currentTarget.value.length;
            var textRemaining = charactersLimit - textLength;
            this.$el.find('#count').text(textRemaining);
        },
        serialize: function() {
            return {
                maxLength: charactersLimit,
                charactersRemaining: charactersLimit
            }
        }  
    });
    
    var Component = UIComponent.extend({
        id: 'textlimit',
        dataTypes: ['TEXT'],
        Input: Input
    });
    
    
    return Component;
});
```

## Enabling an UI
To use this UI just copy this file into the directory **ui** in the root fo the directus's directory.