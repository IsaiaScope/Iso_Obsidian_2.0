- [Class component](https://react.dev/reference/react/Component) => no more necessari after React v16.8 when hook are introduced
	- prop = this.props
	- need super()
	- render() {}
	- this.toggleUsersHandler.bind(this) => to bind jsx function in template
	- managing state
		- instead useState(), initialize state in constructor this.state() , and declare a normal method where use this.setState() to update the state
	- lifecycle
		- [[class-component-react.png]]

ex. class component
```jsx
import { Component } from 'react';

import User from './User';
import classes from './Users.module.css';

class Users extends Component {
  constructor() {
    super();
    this.state = {
      showUsers: true,
      more: 'Test',
    };
  }

  componentDidUpdate() {
    // try {
    //   someCodeWhichMightFail()
    // } catch (err) {
    //   // handle error
    // }
    if (this.props.users.length === 0) {
      throw new Error('No users provided!');
    }
  }

  toggleUsersHandler() {
    // this.state.showUsers = false; // NOT!
    this.setState((curState) => {
      return { showUsers: !curState.showUsers };
    });
  }

  render() {
    const usersList = (
      <ul>
        {this.props.users.map((user) => (
          <User key={user.id} name={user.name} />
        ))}
      </ul>
    );

    return (
      <div className={classes.users}>
        <button onClick={this.toggleUsersHandler.bind(this)}>
          {this.state.showUsers ? 'Hide' : 'Show'} Users
        </button>
        {this.state.showUsers && usersList}
      </div>
    );
  }
}

export default Users;
```
