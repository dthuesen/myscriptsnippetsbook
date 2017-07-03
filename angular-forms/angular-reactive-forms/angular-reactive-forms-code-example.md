# Angular Reactive Forms Code Example

## The HTML form

The starting point is an example with a basic form:

_**customer.component.html**_

```js
<form>
    <fieldset>
        <div>
            <label for="firstNameId">First Name</label>
            <input id="firstNameId" type="text"
                   placeholder="First Name (required)"
                   required
                   minLength="3" />
        </div>
        <button type="submit">Submit</button>
    </fieldset>
</form>
```

Transformed into a **reactive form** in the template it would look like this:

