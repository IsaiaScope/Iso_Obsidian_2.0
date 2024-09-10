- [DOC of Class components](https://react.dev/reference/react/Component) => no more necessari after React v16.8 when hook are introduced
	- prop = this.props
	- need super()
	- render() {}
	- this.toggleUsersHandler.bind(this) => to bind jsx function in template
	- managing state
		- instead useState(), initialize state in constructor this.state() , and declare a normal method where use this.setState() to update the state
	- lifecycle
		- [[class-component-react.png | IMG of class component flow]]

- ex. class component
	- [GitHub Repo of class-based-cmp](https://github.com/academind/react-complete-guide-code/tree/13-class-based-cmp/code/08-finished/src)
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

// const Users = () => {
//   const [showUsers, setShowUsers] = useState(true);

//   const toggleUsersHandler = () => {
//     setShowUsers((curState) => !curState);
//   };

//   const usersList = (
//     <ul>
//       {DUMMY_USERS.map((user) => (
//         <User key={user.id} name={user.name} />
//       ))}
//     </ul>
//   );

//   return (
//     <div className={classes.users}>
//       <button onClick={toggleUsersHandler}>
//         {showUsers ? 'Hide' : 'Show'} Users
//       </button>
//       {showUsers && usersList}
//     </div>
//   );
// };

export default Users;
```

```jsx
import { Fragment, useState, useEffect, Component } from 'react';

import Users from './Users';
import classes from './UserFinder.module.css';
import UsersContext from '../store/users-context';
import ErrorBoundary from './ErrorBoundary';

class UserFinder extends Component {
  static contextType = UsersContext;

  constructor() {
    super();
    this.state = {
      filteredUsers: [],
      searchTerm: '',
    };
  }

  componentDidMount() {
    // Send http request...
    this.setState({ filteredUsers: this.context.users });
  }

  componentDidUpdate(prevProps, prevState) {
    if (prevState.searchTerm !== this.state.searchTerm) {
      this.setState({
        filteredUsers: this.context.users.filter((user) =>
          user.name.includes(this.state.searchTerm)
        ),
      });
    }
  }

  searchChangeHandler(event) {
    this.setState({ searchTerm: event.target.value });
  }

  render() {
    return (
      <Fragment>
        <div className={classes.finder}>
          <input type='search' onChange={this.searchChangeHandler.bind(this)} />
        </div>
        <ErrorBoundary>
          <Users users={this.state.filteredUsers} />
        </ErrorBoundary>
      </Fragment>
    );
  }
}

// const UserFinder = () => {
//   const [filteredUsers, setFilteredUsers] = useState(DUMMY_USERS);
//   const [searchTerm, setSearchTerm] = useState('');

//   useEffect(() => {
//     setFilteredUsers(
//       DUMMY_USERS.filter((user) => user.name.includes(searchTerm))
//     );
//   }, [searchTerm]);

//   const searchChangeHandler = (event) => {
//     setSearchTerm(event.target.value);
//   };

//   return (
//     <Fragment>
//       <div className={classes.finder}>
//         <input type='search' onChange={searchChangeHandler} />
//       </div>
//       <Users users={filteredUsers} />
//     </Fragment>
//   );
// };

export default UserFinder;
```

```jsx
import { Component } from 'react';

import classes from './User.module.css';

class User extends Component {
  componentWillUnmount() {
    console.log('User will unmount!');
  }

  render() {
    return <li className={classes.user}>{this.props.name}</li>;
  } 
}

// const User = (props) => {
//   return <li className={classes.user}>{props.name}</li>;
// };

export default User;
```

```jsx
import { Component } from 'react';

class ErrorBoundary extends Component {
  constructor() {
    super();
    this.state = { hasError: false };
  }

  componentDidCatch(error) {
    console.log(error);
    this.setState({ hasError: true });
  }

  render() {
    if (this.state.hasError) {
      return <p>Something went wrong!</p>;
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```

```jsx
import React from 'react';

const UsersContext = React.createContext({
  users: []
});

export default UsersContext;
```

```jsx
import UserFinder from './components/UserFinder';
import UsersContext from './store/users-context';

const DUMMY_USERS = [
  { id: 'u1', name: 'Max' },
  { id: 'u2', name: 'Manuel' },
  { id: 'u3', name: 'Julie' },
];

function App() {
  const usersContext = {
    users: DUMMY_USERS
  }

  return (
    <UsersContext.Provider value={usersContext}>
      <UserFinder />
    </UsersContext.Provider>
  );
}

export default App;
```

```jsx
import { Component } from 'react';

class ErrorBoundary extends Component {
  constructor() {
    super();
    this.state = { hasError: false };
  }

  componentDidCatch(error) {
    console.log(error);
    this.setState({ hasError: true });
  }

  render() {
    if (this.state.hasError) {
      return <p>Something went wrong!</p>;
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```
