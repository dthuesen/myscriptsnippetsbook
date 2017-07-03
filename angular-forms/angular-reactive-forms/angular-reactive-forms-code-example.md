# Angular Reactive Forms Code Example

## HTML form

_customer.component.html_

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



