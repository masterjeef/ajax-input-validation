# Ajax Input Validation

This tool is great for validating a single input via ajax. This tool is not intended to replace all validation. A great example usage would be the availability of a username. This likely requires a serverside check against the database. Idealy

I created this because I was working on a web appliaction where some custom field validation existed on both the frontend and backend. The purpose of this tool is to consolidate validation to the backend.

Also, there are some scenarios where you need the validation to occur in the backend. For example, if your validation requires a hit to the database, or some kind of server side action.

## How To

First, include `js/ajax-forms.js` like any other Jquery extension using the `script` tag.

#### The Input To Validate

    <input type="text"
           name="username"
           class="form-control"
           id="exampleInputUsername"
           placeholder="Username"
           validate-ajax="http://localhost:2300/api/validate/username"
           spinner="usernameSpinner"
           success="usernameSuccess"
           danger="usernameDanger">
        
**name** is the name of the URL encoded parameter

**validate-ajax** is the endpoint for the validation

**spinner** is the id of the spinner/loading element

**success** is the id of the success icon/element

**danger** is the id of the danger icon/element

Note: the default hidden class happens to be `hidden`. This can be easily configured, along with all the other default functionality.

##### The Spinner

It's nice to provide some kind of feedback while the input is being validated. The spinner can be any kind of element. My example is using the spinner in Font Awesome.

    <i id="usernameSpinner" class="fa fa-spinner fa-spin hidden"></i>

##### Success

This is what you want to show when the input entered is valid.

    <i id="usernameSuccess" class="fa fa-check-circle hidden"></i>

##### Danger

This is what you want to show when the input is not valid. Notice that danger element also uses the Bootstrap popover.

    <i id="usernameDanger"
       class="fa fa-exclamation-circle hidden"
       data-toggle="popover"
       data-placement="bottom"
       data-trigger="click"
       data-container="body"
       title="Username Not Valid"
       data-content="Username not available"></i>


That's all for the front end. Toggling visibility for all these elements, and generating the ajax request is all handled by the plugin.

#### The Validation Endpoint (Server side)

##### The Request

The **name** attribute is used to name the URL encoded parameters. Therefore the username example above would generate the following GET request : 

`http://localhost:2300/api/validate/username?username={valueEntered}`

##### The Response

If you wish to use the default input validation functions, the validation endpoints must return json in the following format :

    {
        "isValid" : true, // a boolean for the validity of the field
        "message" : "The resulting message" // a helpful message if the field is not valid
                                            // NOTE: this message is used in the danger popover
    }

