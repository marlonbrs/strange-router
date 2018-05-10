# strange-router

A module to aid bringing configurable routes to react-router v4

Lead Maintainer - [William Woodruff](https://github.com/wswoodruff)

## Usage

> See also the [API Reference](API.md)

```bash
npm install strange-router
```

### Config

```js
const routes = [
    {
        path: 'first', // matches '/first'
        component: MyComponent,
        childRoutes: [
            {
                path: 'second', // matches '/first/second'
                childRoutes: [
                    { path: 'third' }, // matches '/first/second/third'
                    { redirect: { to: '/404' } } // Catch all, redirects to absolute path '/404'
                ]
            },
            { redirect: { from: 'oldPath', to: 'newPath' } } // '/first/oldPath' will redirect to '/first/newPath'
            {
                path: 'newPath',
                component: NewStuff,
                onWillMount: ({ route, match, location, history }) => {

                    fetchSomeData();
                },
                onDidMount: ({ route, match, location, history }) => {

                    logSomething();
                },
                onWillUnmount: ({ route, match, location, history }) => {

                    cleanup();
                },
                componentDidCatch: ({ err, info, route, match, location, history }) => {

                    handleErr(err);
                }
            }
        ]
    },
    {
        path: '404',
        component: FourOFour
    }
];
```

### Integration (w/ redux)
```js
const React = require('react');
const { Provider } = require('react-redux');
const StrangeRouter = require('strange-router');
const { ConnectedRouter } = require('react-router-redux');

class App extends React.PureComponent {

    static propTypes = {
        history: T.object.isRequired,
        routes: T.array.isRequired,
        store: T.object.isRequired
    }

    render() {

        const { history, routes, store } = this.props;

        return (
            <Provider store={store}>
                <ConnectedRouter history={history}>
                    {StrangeRouter.buildRoutes(routes)}
                </ConnectedRouter>
            </Provider>
        );
    }
};
```
