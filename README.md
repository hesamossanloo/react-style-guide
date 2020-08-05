# react-style-guide

## Git

- Use [Github Flow](https://guides.github.com/introduction/flow/)
- Prefer a clean history with rebase rather than merge

## JSX readability

### Styling

Avoid inline styling

```JSX
// Bad
<div style={display:'absolute', top: 30, …}>…</div>

// Good
import styles from './styles'
// or
const styles = {…}
…
<div style={styles.meaningfulName}>…</div>
```

### Handler names

When reading JSX it should be clear what will happen on events

```JSX
// Bad
<Button onClick={handleClick}>Go</Button>
// Good
<Button onClick={saveDocument}>Go</Button>
```

Sometimes this isn't practical

```JSX
// Bad
<Button onClick={createOrSaveDocOrLogin}>Save</Button>

// Better
const handleClick = () => {
  if (!loggedIn) {
    goToLogin()
  } else if (isNew) {
    create()
  } else {
    save()
  }
}
return <Button onClick={handleClick}>Save</Button>

// Good
…
return <Button onClick={handleSave}>Save</Button>
```

### Process data outside JSX

```JSX
// Bad
<Button onClick={someBool ? someAction : otherAction} size={Math.max(this/that, minButtonSize)}>Save</Button>

// Good
const save = someBool ? someAction : otherAction
const size = Math.max(this/that, minButtonSize)
return <Button onClick={save} size={size}>Save</Button>
```

### Visible state naming

By using "open/close" instead of "show/hide" we can avoid the ambiguity of 'show' being both a state (`show=true`) and an action (`show()`)

- State: thingOpen true/false
- Action: openThing
- Action: closeThing
- Action: toggleThing

e.g.

```js
const [modalOpen, setModalOpen] = useState(false)
const openModal = () => setModalOpen(true)
…
```

## Folder Structure
### React Components
Some of the key files in a React app
* src
  * index.js - includes `ReactDOM.render(rootComponent, rootElement)` and little else
  * app - react stuff
    * components - Reusable components, building blocks - buttons, modals, etc
      * ComponentA
        * index.js - `export { default } from './ComponentA'`
        * ComponentA.js
        * style.js
      * messaging - an example of components which make sense to be grouped
        * index.js - `export { default as Message } from './Message'` etc
        * Message.js
        * MessageButton.js
        * MessageBar.js
        * MessageList.js
        * style.js
    * pages - Main views in the app - home page, signup, etc
    * modals - Specific modals, if any - e.g. settings modal
    * hooks - shared hooks
    * store - redux reducers, initial state, selectors, etc.
    * i18n - Setup & language files
    * App.js - root component, may include Redux Provider, Router & root Routes, etc
    * config.js
  * img - imported assets
* public - static assets not imported in JS

Don't strictly use this structure if it would be more tidy to deviate from it.
* If you have many small, simple components it adds unnecassary complexity to put each one in a folder with an `index.js` and a `styles.js`. 
* If you have not just images, but videos, sounds, etc, it might be better to have an `assets` folder and move `img` inside that.
* Maybe 'pages' and 'modals' aren't the only high-level structures in the app. You might need to create other folders like `sidebars` or `overlays`
