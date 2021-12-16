# 20. React, Part II

## Advanced React

```js
//CopyCat.js
import React from 'react';
import {styles} from '../styles';
import PropTypes from 'prop-types';

const images = {
  copycat: 'https://content.codecademy.com/courses/React/react_photo_copycat.png',
  quietcat: 'https://content.codecademy.com/courses/React/react_photo_quietcat.png'
};


export class CopyCat extends React.Component {
  render() {
    const { copying, toggleTape, value, handleChange, name } = this.props;
    
    return (
      <div style={styles.divStyles}>
        <h1 style={{marginBottom: 80}}>Copy Cat {name || 'Tom'}</h1>
        <input type='text'
               value={value}
               onChange={handleChange}/>
        <img 
          style={styles.imgStyles}
          alt='cat'
          src={copying ? images.copycat : images.quietcat}
          onClick={toggleTape}
        />
        <p>
          {copying && value}
        </p>
      </div>
    );
  };
}

CopyCat.propTypes = {
  copying:      PropTypes.bool.isRequired,
  toggleTape:   PropTypes.func.isRequired,
  value:        PropTypes.string.isRequired,
  handleChange: PropTypes.func.isRequired,
  name:         PropTypes.string
}
```

```js
import React from 'react';
import { CopyCat } from '../components/CopyCat';

export default class CopyCatContainer extends React.Component {
    constructor(props) {
    super(props);

    this.state = { 
      copying: true,
      input: ''
    };

    this.handleChange = this.handleChange.bind(this);
    this.toggleTape = this.toggleTape.bind(this);
  }

  handleChange(e) {
    this.setState({input: e.target.value});
  }

  toggleTape() {
    this.setState({copying: !this.state.copying})
  }
  
  render() {
    const copying = this.state.copying;
    const toggleTape = this.toggleTape;
    const input = this.state.input;
    const handleChange = this.handleChange;
    
    return (
      <CopyCat copying={copying}
               toggleTape={toggleTape}
               value={input}
               handleChange={handleChange}
               name='Tojo'/>
    );
  };
}
```

```js
//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import CopyCatContainer from './containers/CopyCatContainer';

ReactDOM.render(
  <React.StrictMode>
    <CopyCatContainer />
  </React.StrictMode>,
  document.getElementById('root')
);
```

```js
//styles.js
const fontFamily = 'Comic Sans MS, Lucida Handwriting, cursive';
const fontSize = '5vh';
const backgroundColor = '#282c34';
const minHeight = '100vh';
const minWidth = 400;
const display = 'flex';
const flexDirection = 'column';
const alignItems = 'center';
const justifyContent = 'center';
const color = 'white';
const marginTop = '20px';
const width = '50%';

const divStyles = {
  fontFamily: fontFamily,
  fontSize: fontSize,
  color: color,
  backgroundColor: backgroundColor,
  minHeight: minHeight,
  minWidth: minWidth,
  display: display,
  flexDirection: flexDirection,
  alignItems: alignItems,
  justifyContent: justifyContent,
};

const imgStyles = {
  marginTop: marginTop,
  width: width
};

export const styles = {
  divStyles: divStyles,
  imgStyles: imgStyles
}
```



## Share Styles Across Multiple Components

```js
//styles.js
const fontFamily = 'Comic Sans MS, Lucida Handwriting, cursive';
const fontSize = '5vh';
const backgroundColor = '#282c34';
const minHeight = '100vh';
...

const divStyles = {
  fontFamily: fontFamily,
  fontSize: fontSize,
  color: color,
...
};

const imgStyles = {
  marginTop: marginTop,
  width: width
};

export const styles = {
  divStyles: divStyles,
  imgStyles: imgStyles
}
```

```js
//CoyCat.js
...
import {styles} from '../styles';
...
```

## ==Container Components from Presentational Components==

*   In this programming pattern, the container component does the work of figuring out what to display. The presentational component does the work of actually displaying it. If a component does a significant amount of work in both areas, then that’s a sign that you should use this pattern!
*   <img src="../../../../../../../../../../Library/Application%20Support/typora-user-images/image-20211213100424172.png" alt="image-20211213100424172" style="zoom: 33%;" />

## Proptypes

*   `propTypes` are useful for two reasons. 
    *   The first reason is *prop validation*.
        *   *Validation* can ensure that your `props` are doing what they’re supposed to be doing. **If `props`are missing**, **or** if they’re **present** but they **aren’t what you’re expecting**, then **a warning will print in the console.**
    *   Reason #2 is arguably more useful: *documentation*.
        *   *Documenting* `props` makes it easier to glance at a file and **quickly understand the component class inside**. When you have a lot of files, and you will, this can be a huge benefit.

```js
CopyCat.propTypes = {
  copying:      PropTypes.bool.isRequired,
  toggleTape:   PropTypes.func.isRequired,
  value:        PropTypes.string.isRequired,
  handleChange: PropTypes.func.isRequired,
  name:         PropTypes.string
}
```

## React Forms

*   In a typical, non-React environment, a **user types some data into a form’s input fields**, and the **server doesn’t know about it.** 
    *   User remains clueless until "submit" button is clicked.
*   In a React form, you want the server to know **every new character or deletion *as soon as it happens.***

### Input onChange

```react
//CopyCat.js
...
return (
	<input type='text'
           value={value}
           onChange={handleChange}/>
...
)
...
```

### Write an Input Event Handler

*   An event handler is a function that gets called whenever a user enters or deletes any character.

*   ```react
    //CopyCatContainer.js
    ...
    handleChange(e) {
        this.setState({input: e.target.value});
    }
    ```

### Controlled vs Uncontrolled Components

*   An *uncontrolled component* is a component that maintains its own internal state.
*   A *controlled component* is a component that does not maintain any internal state.
    *   Since a controlled component has no state, it must be *controlled* by someone else.
*   In React, **when you give an `<input />` a `value` attribute**, then something strange happens: the **`<input />`BECOMES controlled**.
    *   **It stops using its internal storage.**
    *   ==This is a more ‘React’ way of doing things.==

