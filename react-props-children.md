#What even is ‘children’?

The React docs say that you can use props.children on components that represent ‘generic boxes’ and that ‘don’t know their children ahead of time’. For me, that didn’t really clear things up. I’m sure for some, that definition makes perfect sense but it didn’t for me.

My simple explanation of what this.props.children does is that it is used to display whatever you include between the opening and closing tags when invoking a component.

This de-couples the <Picture> component from its content and makes it more reusable.

React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.
So it support composition

#Example 

```js
const Picture = (props) => {
  return (
    <div>
      <img src={props.src}/>
      {props.children}
    </div>
  )
}

//App.js
render () {
  return (
    <div className='container'>
      <Picture key={picture.id} src={picture.src}>
          //what is placed here is passed as props.children  
      </Picture>
    </div>
  )
}

```

#example 2

```
//App.js
import React, { Component } from 'react';
import Picture from './Picture';
import Button from './Button';
class App extends Component {
  constructor(props) {
    super(props);
this.state = {
      pictures: [
        {id: 1, src: 'http://via.placeholder.com/200x100'},
        {id: 2, src: 'http://via.placeholder.com/400x200'},
        {id: 3, src: 'http://via.placeholder.com/200x100'}
      ],
      currentPic: null
    };
this.setCurrentPic = this.setCurrentPic.bind(this);
  }
setCurrentPic(id) {
    this.setState({currentPic: id});
  }
render () {
    return (
      <div>
        <div className='squares'>
          {this.state.pictures.map((picture) => {
            return (
              <Picture key={picture.id} src={picture.src}>
                <Button
                  pictureSrc={picture.src}
                  setCurrentPic={this.setCurrentPic}
                  id={picture.id}
                />
              </Picture>
            )
          })}
        </div>
        <div>
          <p>Current selected picture ID is {this.state.currentPic}</p>
        </div>
      </div>
    )
  }
}
export default App;
//Picture.js
import React, { Component } from 'react';
const Picture = (props) => {
  return (
    <div className='picture'>
      <img src={props.src} className='picture'/>
      {props.children}
    </div>
  )
}
export default Picture;
//Button.js
import React, { Component } from 'react';
class Button extends Component {
  constructor(props) {
    super(props);
    this.state = {
      pictureId: null,
      label: null
    };
    this.buttonLabel = this.buttonLabel.bind(this);
  }
buttonLabel(src) {
    src.includes('200x100')
    ? this.setState({pictureId: this.props.id, label: 'Small'})
    : this.setState({pictureId: this.props.id, label: 'Large'})
  }
componentDidMount() {
    this.buttonLabel(this.props.pictureSrc)
  }
render() {
    return (
      <div>
        <button
          onClick={() => this.props.setCurrentPic(this.props.id)}
        >
          {this.state.label}
        </button>
      </div>
    )
  }
}
export default Button;
```
